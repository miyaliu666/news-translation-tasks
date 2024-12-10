---
title: 如何使用 R Shiny 构建一个天气应用
date: 2024-12-10T05:26:01.983Z
author: Elabonga Atuo
authorURL: https://www.freecodecamp.org/news/author/Ellabee/
originalURL: https://www.freecodecamp.org/news/how-to-build-a-weather-app-with-r-shiny/
posteditor: ""
proofreader: ""
---

在本教程中，你将学习如何在 R 中构建一个天气应用。真的——用 R 构建一个天气应用？等一下，听我说完。

<!-- more -->

当你想到 R 时，你可能会想象一个戴着厚厚的处方眼镜、沉浸于书本的人。你知道的，一个与复杂模型、庞大数学方程和大量数据打交道的统计人员。

但 R 远不止只是一个统计工具。当你需要将原始数据转化为可操作的洞察，并将这些洞察以清晰、引人入胜的方式呈现时，R 会闪闪发光。

有了像 Shiny 这样的框架，R 更进一步，让你无需担心前端、后端或学习全新的编程语言就能创建完全交互式的 Web 应用。

在本教程中，你将创建一个简单的天气应用，它从 API 获取数据并在一个美观的应用中显示结果。

## 目录

1.  [项目概述][1]
    
2.  [项目设置][2]
    
3.  [API 密钥：存储和检索][3]
    
4.  [如何进行第一次 API 调用][4]
    
5.  [如何构建 Shiny 应用][5]
    
6.  [总结][6]
    

## 项目概述

以下是我们将要构建的内容：

![R Shiny 天气应用演示](https://cdn.hashnode.com/res/hashnode/image/upload/v1733341336823/dd605385-5531-43c5-924d-dde24b38846b.gif)

要使天气应用正常工作，你需要进行两个独立的 API 调用。我们将使用 One Call API 3.0 来更新天气数据，使用 OpenWeather API 进行地理编码。你可以在[这里][7]获取你的 API Key。需要注意的是，如果这是你第一次申请 API 密钥，激活可能需要长达 24 小时。

天气应用将从用户输入中获取位置/城市。该输入将通过调用 OpenWeather API 进行地理编码。然后，从其响应中提取坐标（纬度和经度）。这些坐标将用作 One Call API 调用的查询参数，以获得 JSON 格式的天气数据。

### 前提条件：

要跟随本教程，你需要：

-   R 编程知识
    
-   HTML 和一点 JavaScript 知识
    
-   已安装 R Studio
    

![天气更新 API 流程](https://cdn.hashnode.com/res/hashnode/image/upload/v1733172415724/c4f884f6-b583-4f13-b0f8-eb564ab6531f.png)

## 项目设置

在你想要的目录中创建一个文件夹。使用以下命令在 R 控制台中设置并确认项目文件夹为工作目录：

```
setwd("path/to/your/project/file")
getwd()
```

在设置的路径下创建一个项目，使用以下命令：

```
#create R project
usethis::create_project(path = ".", open = FALSE)
```

你应该拥有如下结构的文件夹。

![项目文件夹结构](https://cdn.hashnode.com/res/hashnode/image/upload/v1733166096334/93e004da-4449-4cb4-8ddd-d3082e5687d8.png)

在根目录中创建一个 R 文件，并将其保存为 `app.R`。你的所有 R 代码将保存在这里。

安装并加载你将要使用的以下库：

```
library(shiny)
library(bslib)
library(shinyjs)
library(httr2)
library(lubridate)
library(shiny.semantic)
```

## API 密钥：存储和检索

将你的凭据存储在与脚本和全局环境分开的地方是个好习惯。这确保了安全性、可扩展性和灵活性，尤其是在共享或生产环境中工作时。`.Renviron` 文件最好用于此目的。

以以下方式打开并编辑你的 `.Renviron` 文件：

```
#open and edit .Renviron
usethis::edit_r_environ(scope=c("project")
```

将范围参数设置为 `project` 专门针对你的项目设置 `.Renviron`。在新打开的文件中，添加你的 API 密钥如下：

```
OPENWEATHERAPIKEY="yourapikey"
```

## 如何进行第一次 API 调用

你将使用 httr2 库（基于 httr 构建）从 API 获取数据。它为你提供了更多的控制权去如何向网络发出请求。

### 在脚本中使 API 密钥可访问

首先，你需要在不硬编码的情况下安全地访问和存储脚本中的 API 密钥。你可以这样做：

```
#access API keys in script
readenviron(".Renviron")
api_key = Sys.getenv("OPENWEATHERAPIKEY")
```

### 定义地理编码函数

你将创建一个函数，接受位置和 API 密钥作为输入，向 OpenWeather 地理编码 API 发送请求，并返回指定位置的坐标。

首先创建一个请求。管道符号 (`|>`) 操作符简化了 HTTP 请求逐步链接的方式，使其更加清晰和易读。地理编码 URL 接受两个参数：位置，以 `q` 表示，以及 API 密钥，以 `app_id` 表示。`req_url_query()` 函数将这些参数附加到查询中。

链接查询以执行请求并获取操作，最后使用倒数第二行获取 JSON 格式的响应。
```

![地理编码 API 的示例响应](https://cdn.hashnode.com/res/hashnode/image/upload/v1733342454801/feed01b2-a7a1-4c69-8297-2dcfdc8ec39f.png)

### 定义坐标提取函数

`coordinates()` 函数是一个辅助函数，用于从 JSON 响应中提取纬度和经度值。对 JSON 响应的一次快速检查即可揭示坐标的位置。JSON 对象实际上是一个长长的列表，您可以通过子集访问元素。

一个空的数据体表示城市/位置不可用，您将收到消息 _"No such city exists!"_ 。如果 JSON 包含一个元素，则长度会大于 0 —— 毕竟它是一个列表。

```
coordinates <- function(body) {
  if(length(body) != 0) { 
    lat <- body$coord$lat
    lng <- body$coord$lon
    town <- body$name
    c(lat, lng, town)
  } else {
    "No such city exists!"
  }
}
```

### 定义天气更新函数

您将创建一个函数，发送请求到 OpenWeather API，使用指定的查询参数，利用预定义的函数处理错误，并返回解析后的包含天气数据的 JSON 响应。

如地理编码函数中实现的那样，首先创建一个请求，并使用 `req_url_query()` 函数添加必要的查询参数。`openweather_json()` 函数接受两个主要参数：

-   `api_key`: 这是一个必需参数，用于与 OpenWeather API 的认证，通过位置匹配。
    
-   `...`: 这表示可选的关键字参数，您可以用来定制查询。只要它们被指定为具名参数，您可以传递任意多的额外参数。
    

```
openweather_json <- function(api_key, ...) { 
  request(current_weather_url) |> 
    req_url_query(..., `appid` = api_key, `units` = "metric") |> 
    req_error(body = openweather_error_body) |>
    req_perform() |> 
    resp_body_json()
}
```

### 错误处理：提取和管理状态码

您将创建一个错误处理函数，该函数从响应中提取非 200 的状态码，并定义如何管理它们。此函数的结构取决于 API 如何报告错误以及相关信息存储在哪里。

#### 定义天气更新错误体

`openweather_json()` 中的 `req_error()` 引入了一个新概念：错误处理。API 请求可能抛出异常，获取状态码有助于了解应向用户显示什么消息以及如何解决。

创建一个错误体，这是一个函数，如果状态码不是 200（表示一切正常），则捕获错误码。

该函数接收一个响应并从 JSON 响应中提取存储在 `$message` 子列表中的状态响应。下划线 `_` 是 JSON 对象的占位符。

```
openweather_error_body <- function(resp) {
  resp |> resp_body_json() |> _$message 
}
```

#### 定义地理编码错误体

此错误体函数在 Shiny 应用程序中将非常有用。这是一个简单的示例。

`req_error()` 函数允许您自定义如何处理响应错误。它的 `is_error` 参数决定给定的响应是否应视为错误。通过将 `is_error` 设置为 `\(resp) FALSE`（一个始终返回 FALSE 的匿名函数），所有响应，无论状态码如何，都被视为成功。这可防止应用程序因非 200 状态码而退出。

通过此设置，您可以从响应体中提取状态码，并将其传入 `resp_status()` 函数以检索确切的代码。

```
openstreetmap_error_body <- function(location, api_key) {
  resp <- request(geocoding_url) |> 
    req_url_query(`q` = location, `appid` = api_key) |> 
    req_error(is_error = \(resp) FALSE) |>
    req_perform() |>  resp_status()
  resp
}
```

## 如何构建 Shiny 应用

现在您已经找到如何从 API 获取数据的方法，接下来就是以可解释和交互的格式呈现结果。为此，您将使用 Shiny。Shiny 是一个允许您创建交互式 Web 应用程序的框架。

一个 Shiny 应用程序由两个组件组成：

-   UI：用户交互的界面。它定义了应用程序的布局和外观。
    
-   服务器：包含应用程序的逻辑和行为。
    

### 构建 Shiny UI

Shiny UI 提供了一系列元素，允许用户输入数据、做出选择并无缝触发事件。

您将包括一个 `textInput` 元素来接收位置并在提交时获取和呈现天气数据。`input_task_button` 按钮可防止在 API 调用正在进行时用户点击。其他元素是天气数据将被显示的输出元素和一个模式切换按钮。

#### 为 Shiny 应用程序设计样式

您可以使用 `shiny.semantic`，一个基于 Fomantic-UI 构建的库，为您的 Shiny 仪表板设计样式。Fomantic-UI 是一个前端框架，提供丰富的预设计 HTML 组件集合，如按钮、模态框、表单输入等。它通过让开发者无需广泛的自定义 CSS 或 HTML 知识，即可创建视觉吸引力和响应式的界面，从而简化了 UI 设计。

Fomantic-UI 中的网格是一个灵活的布局系统，用于组织内容。它充当画布，将布局划分为行（水平对齐）和列（垂直对齐）。根网格最多可以包含 16 列，非常适合创建结构化和响应式的设计。

要指定列的宽度，可以附加宽度类和大小（一个从 1 到 16 的数字）来表示它的范围。行中所有列的总宽度应加起来为 16。

段落用于分组相关内容，而卡片用于显示详细、信息丰富的项目，例如用户的社交媒体资料。分隔线是用于分隔布局内不同部分或内容的视觉元素。

对于天气应用程序，首先创建一个类为 `grid` 的 div，其中嵌套不同的元素。

![渐进式页面布局演示](https://cdn.hashnode.com/res/hashnode/image/upload/v1733137762676/12d5695c-2ed7-4606-8267-44243c2bee57.png)

##### **搜索栏部分**

将网格分为十六列，并创建一个段落以分组搜索栏部分中的元素。添加主题切换按钮、用于接收用户输入的位置输入、将位置提交给 API 的搜索按钮，以及通知按钮，并通过列大小定义它们的宽度。

```
div(class = "sixteen wide column",
          div(class = "ui segment",
              div(class = "ui grid",
                  div(class = "two wide column",
                      button(
                        class = "ui button icon basic",
                        input_id = "darkmode",
                        label = NULL,
                        icon = icon("moon icon")
                      )
                  ),
                  div(class = "ten wide column",
                      textInput(
                        "location",
                        label = NULL,
                        placeholder = "Search for your preferred city"
                      )
                  ),
                  div(class = "two wide column",
                      tags$div(
                        class = "ui button",
                        id = "my-custom-button",
                        input_task_button("search", label = "Search", icon = icon("search"))
                      )
                  ),
                  div(class = "two wide column",
                      actionButton("show_alert", label = icon("bell"), class = "bell-no-alert"),
                      textOutput("alert_message")
                  )
              )
          )
      )
```

##### **位置和当前天气部分**

将网格划分为十六列，并在分区内嵌套另一个网格，该网格将容纳两列。

在网格中定义两列。第一列用于时间、位置和日期数据，第二列将容纳当前天气数据。

然后创建卡片元素，以包含每个天气参数、其测量单位以及相应的图标。

```
div(class = "sixteen wide column",
          div(class = "ui equal-height-grid grid",
              div(class = "left floated center aligned four wide column",
                  div(class = "ui raised equal-height-two-segment segment",
                      style = "flex: 1;",
                      div(class = "column center aligned",
                          div(class = "ui hidden section divider"),
                          span(class = "ui large text", textOutput("city")),
                          div(class = "ui hidden section divider"),
                          span(class = "ui big text", textOutput("currentTime")),
                          div(class = "ui hidden section divider"),
                          span(class = "ui large text", textOutput("currentDate")),
                          div(class = "ui hidden section divider")
                      )
                  )
              ),
              div(class = "right floated center aligned twelve wide column",
                  div(class = "ui raised segment",
                      div(class = "ui horizontal equal width segments",
                          div(class = "ui equal-height-two-segment segment",
                              style = "flex: 3;",
                              div(class = "column",
                                  span(class = "ui big text centered", textOutput("currentTemp")),
                                  textOutput("feelsLike"),
                                  card(
                                    class = "ui mini",
                                    div(class = "content", icon(class = "large sun"),
                                        div(class = "sub header", "日出"),
                                        div(class = "description", textOutput("sunriseTime"))
                                    )
                                  ),
                                  card(
                                    class = "ui mini",
                                    div(class = "content", icon(class = "large moon"),
                                        div(class = "sub header", "日落"),
                                        div(class = "description", textOutput("sunsetTime"))
                                    )
                                  )
                              )
                          ),
                          div(class = "ui segment",
                              style = "flex: 3;",
                              div(
                                class = "column center aligned",
                                div(class = "ui hidden divider"),
                                htmlOutput("currentWeatherIcon"),
                                span(class = "ui large text", textOutput("currentWeatherDescription"))
                              )
                          ),
                          div(class = "ui segment",
                              style = "flex: 3;",
                              div(class = "column",
                                  card(
                                    class = "ui tiny",
                                    div(class = "content", icon(class = "big tint"),
                                        div(class = "sub header", "湿度"),
                                        div(class = "description", textOutput("currentHumidity"))
                                    )
                                  ),
                                  card(
                                    class = "ui tiny",
                                    div(class = "content", icon(class = "big tachometer alternate"),
                                        div(class = "sub header", "压力"),
                                        div(class = "description", textOutput("currentPressure"))
                                    )
                                  )
                              )
                          ),
                          div(class = "ui segment",
                              style = "flex: 3;",
                              div(class = "column center aligned",
                                  card(
                                    class = "ui tiny",
                                    div(class = "content", icon(class = "big wind"),
                                        div(class = "sub header", "风速"),
                                        div(class = "description", textOutput("currentWindSpeed"))
                                    )
                                  ),
                                  card(
                                    class = "ui tiny",
                                    div(class = "content", icon(class = "big umbrella"),
                                        div(class = "sub header", "紫外线指数"),
                                        div(class = "description", textOutput("currentUV"))
                                    )
                                  )
                              )
                          )
                      )
                  )
              )
          )
      )
```

该部分保存预测数据。将网格分为十六列，并在分区中嵌套另一个包含两列的网格。

在网格中，定义两列。第一列保存 _5 日预测_ 数据。使用行分隔不同值的元素。第二列包含 _小时预测_ 数据。使用列分隔不同值的元素。

```
      # 预测部分
      div(class = "sixteen wide column",
          div(class = "ui grid equal-height-grid",
              div(class = "left floated center aligned six wide column",
                  div(class = "ui raised segment special-segment equal-height-segment",
                      h4("5 日预测:"),
                      div(class = "ui three column special-column grid",
                          # 每日预测
                          div(class = "row",
                              div(class = "five wide column", textOutput("dailyDtOne")),
                              div(class = "three wide column", textOutput("dailyTempOne")),
                              div(class = "three wide column", htmlOutput("dailyIconOne"))
                          ),
                          div(class = "row",
                              div(class = "five wide column", textOutput("dailyDtTwo")),
                              div(class = "three wide column", textOutput("dailyTempTwo")),
                              div(class = "three wide column", htmlOutput("dailyIconTwo"))
                          ),
                          div(class = "row",
                              div(class = "five wide column", textOutput("dailyDtThree")),
                              div(class = "three wide column", textOutput("dailyTempThree")),
                              div(class = "three wide column", htmlOutput("dailyIconThree"))
                          ),
                          div(class = "row",
                              div(class = "five wide column", textOutput("dailyDtFour")),
                              div(class = "three wide column", textOutput("dailyTempFour")),
                              div(class = "three wide column", htmlOutput("dailyIconFour"))
                          ),
                          div(class = "row",
                              div(class = "five wide column", textOutput("dailyDtFive")),
                              div(class = "three wide column", textOutput("dailyTempFive")),
                              div(class = "three wide column", htmlOutput("dailyIconFive"))
                          )
                      )
                  )
              ),
              div(class = "right floated center aligned ten wide column",
                  div(class = "ui raised segment special-segment equal-height-segment",
                      h4("小时预测:"),
                      div(
                        class = "ui grid",
                        style = "display: flex; flex-direction: row; align-items: center; justify-content: space-around; flex-wrap: wrap; height: 100%;",
                        # 每小时预测
                        div(class = "column",
                            textOutput("hourlyDtOne"),
                            htmlOutput("hourlyIconOne"),
                            textOutput("hourlyTempOne")
                        ),
                        div(class = "column",
                            textOutput("hourlyDtTwo"),
                            htmlOutput("hourlyIconTwo"),
                            textOutput("hourlyTempTwo")
                        ),
                        div(class = "column",
                            textOutput("hourlyDtThree"),
                            htmlOutput("hourlyIconThree"),
                            textOutput("hourlyTempThree")
                        ),
                        div(class = "column",
                            textOutput("hourlyDtFour"),
                            htmlOutput("hourlyIconFour"),
                            textOutput("hourlyTempFour")
                        ),
                        div(class = "column",
                            textOutput("hourlyDtFive"),
                            htmlOutput("hourlyIconFive"),
                            textOutput("hourlyTempFive")
                        )
                      )
                  )
              )
          )
      )
  )
```

### 构建 Shiny 服务器

UI 部分的每个元素都有一个 ID（唯一标识符），用于操作将显示的数据/信息。

`render*()` 函数集定义了可视化类型，而 `output$*` 函数用于子集元素。这两个用于将可视化与逻辑连接。大多数元素将从 JSON 列表中提取数据，除了天气图标（将引用一个外部链接作为来源）。

#### 反应性

反应性是使 Shiny 应用程序动态化的原因——当依赖项发生变化时，输出会自动更新。

反应性的两个关键组成部分是反应式和观察者。反应式根据其依赖项计算并返回一个值，而观察者监视反应值并运行引起副作用的代码，例如记录日志或更新数据库。

#### 服务器代码

1.  `location` **reactive**

location reactive 包含一个 if-else 条件块，它根据状态码定义要显示的信息。query 变量包含将被地理编码以获取坐标的城市/地点名称。流程通过 `bindEvent()` 管道化，这确保地理编码 API 调用完成后才会进行另一次调用，从而减少不必要的请求。

```
location <- reactive({
    query <- input$location
    if(openstreetmap_error_body(query, api_key) == "404"){
      validate("不存在这样的城市/镇。请检查拼写！")
    }
    else if(openstreetmap_error_body(query, api_key) == "400"){
      validate("错误请求")
    }
    coords <- geocode(query, api_key)
  }) %>% bindEvent(input$search)
```

2.  `weather_data` **reactive**

weather reactive 结合使用从 `location()` 获取和提取的坐标进行地理编码 API 调用和天气更新 API 调用：

```
  weather_data <- reactive({
    loc <- location()
    openweather_json(api_key, lat = loc[1], lon = loc[2])
  })
```

要访问 API 调用返回的 JSON 对象，可以像调用函数一样调用 reactive。接着可以通过子集化 JSON 值来访问要提取的具体值。

```
# 子集化天气数据
  output$city <- renderText({
    location()[3]
  })

  output$currentWeatherDescription <- renderText({
    weather_data()$current$weather[[1]]$description
  })
```

3.  **创建一个解析日期函数**

在 JSON 响应中，所有时间数据，无论是预测的还是当前的，都是以 UNIX 格式提供的。为了让这些信息易于理解，需要将其转换为人类可读的格式。可以通过创建一个函数来实现，该函数以时间数据为输入，并使用 `lubridate` 包中的函数来处理转换。

首先，将时间戳元素转换为 datetime 对象。将时间项格式化为 12 小时制，并将日期项格式化为包含星期几、日期和月份。

-   `%I`：以 12 小时制格式显示小时（01-12）。
    
-   `%M`：显示分钟（00-59）。
    
-   `%p`：添加上午/下午指示符。
    

paste 函数连接这些值。该函数返回一个包含日期和时间值的向量，可以通过子集化来提取。

```
parse_date <- function(timestamp) {
  datetime <- as_datetime(timestamp) 
  date <- paste(weekdays(datetime), ",", day(datetime), months(datetime))
  time <- format(as.POSIXct(datetime), format = "%I:%M %p")
  c(date, time)
}
```

4.  **添加一个显示错误消息的模态框**

location reactive 提供了一种处理错误的方法。您可以通过在发生错误时覆盖页面并禁用其内容，直到用户完成指定的操作，以增强用户体验。

您将添加 JavaScript 以控制模态框的显示方式和时机。

在 UI 部分添加两个模态框，每个模态框都包含错误的解释（标题）和所需操作的概要（内容）。`action` 类包含一个按钮，使用户能够关闭模态框。

```
# 模态框 - UI
  div(id = "notFound", class = "ui modal",
      div(class = "header", "未找到位置"),
      div(class = "content", "不存在这样的城市/镇。请检查拼写！"),
      div(class = "actions",
          div(class = "ui button", id = "closeNotFound", "OK"))
  ),
  div(id = "badRequest", class = "ui modal",
      div(class = "header", "无效请求"),
      div(class = "content", "错误请求。请使用有效详细信息重试。"),
      div(class = "actions",
          div(class = "ui button", id = "closeBadRequest", "OK"))
  )
```

稍微调整 location reactive 以整合模态框。评论掉的代码将被 JavaScript 代码替换。`runjs` 函数根据遇到的错误显示模态框。`req(FALSE)` 终止 reactive 流程。

```
# 显示和隐藏模态框 - 服务器
location <- reactive({
    query <- input$location
    if(openstreetmap_error_body(query, api_key) == "404"){
      #validate("不存在这样的城市/镇。请检查拼写！")
      runjs("$('#notFound').modal('show');")
      req(FALSE)
    }
    else if(openstreetmap_error_body(query, api_key) == "400"){
      #validate("错误请求")
      runjs("$('#badRequest').modal('show');")
      req(FALSE)
    }
    coords <- geocode(query, api_key)
  }) %>% bindEvent(input$search)

# 监听模态框上按钮的点击以隐藏模态框
observeEvent(input$closeNotFound, {
    runjs("$('#notFound').modal('hide');")
  })

observeEvent(input$closeBadRequest, {
    runjs("$('#badRequest').modal('hide');")
  })
```

## 结论

在本教程中，您构建了一个使用 Shiny 构建的天气应用程序，该程序从 API 获取天气数据并以交互且视觉上吸引人的方式显示。

为此，您使用了以下库：

-   `httr2` 用于发送 API 请求并处理响应
    
-   `shiny.semantic` 用于应用程序的样式
    
-   `lubridate` 用于处理和格式化时间数据
    
-   `shinyjs` 用于将 JavaScript 功能集成到应用程序中
    

您可以在[这里][8]找到项目的完整代码。

La Fin!

[1]: #heading-project-overview
[2]: #heading-project-setup
[3]: #heading-api-keys-storage-and-retrieval
[4]: #heading-how-to-make-your-first-api-call
[5]: #heading-how-to-build-the-shiny-app
[6]: #heading-conclusion
[7]: https://openweathermap.org/api
[8]: https://github.com/elabongaatuo/R-weather-app


