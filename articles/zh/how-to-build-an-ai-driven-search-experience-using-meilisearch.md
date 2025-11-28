---
title: 如何使用 Meilisearch 构建 AI 驱动的搜索体验
date: 2025-11-28T05:19:56.997Z
author: Manish Shivanandhan
authorURL: https://www.freecodecamp.org/news/author/manishshivanandhan/
originalURL: https://www.freecodecamp.org/news/how-to-build-an-ai-driven-search-experience-using-meilisearch/
posteditor: ""
proofreader: ""
---

搜索是现代应用程序中最重要的功能之一。用户期待即时答案、有用的建议和符合他们意图的结果，即使他们拼错了单词。

<!-- more -->

大多数传统搜索系统无法在没有复杂且繁重的基础设施的情况下提供这种体验。

[Meilisearch][1] 是一个开源搜索引擎，改变了这种状况，它提供了一个快速且对开发者友好的引擎，易于设置和扩展。

当你将 Meilisearch 与用于自然语言理解和[语义相关性][2]的 AI 模型结合使用时，你可以构建一个强大且直观的搜索体验，感觉现代且智能。

本文解释了 Meilisearch 如何工作，如何设置它，以及如何集成 AI 模型以提供更好的相关性和更好的排名。你还将看到混合搜索和语义嵌入是如何工作的，以及如何使用 Sevalla 将 Meilisearch 部署到云端。

## 我们将覆盖什么

-   [了解 Meilisearch][3]
    
-   [设置 Meilisearch][4]
    
-   [结合使用混合搜索和 AI][5]
    
-   [使用 AI 重写查询][6]
    
-   [使用 Sevalla 将 Meilisearch 部署到云端][7]
    
-   [结论][8]
    

## 了解 Meilisearch

Meilisearch 是一个闪电般快速的搜索引擎，能够轻松地集成到任何应用程序中。

它由 Rust 构建，旨在在不到 50 毫秒的时间内交付结果。它支持语义搜索、混合搜索、容错、排序、地理搜索和多种语言。

Meilisearch 还提供了一个干净的 RESTful API 和多种 SDK，便于与 JavaScript、Python、Go、PHP、Ruby、Rust 及其他多种语言的集成。

你可以通过浏览[官方演示][9]来试用 Meilisearch。这些演示展示了 Meilisearch 不仅限于一个特定的使用案例，而是可以适用于多种工作流程。

Meilisearch 支持两个版本。社区版是完全开源的，基于 MIT 许可证，可以自由使用，即使是商用产品。

企业版引入了诸如[分片][10]这样的特性，并受商业许可证的约束。你可以自行部署 Meilisearch 或选择 Meilisearch 云，它处理托管、更新、监控和分析，无需服务器维护。

## 设置 Meilisearch

启动 Meilisearch 非常简单。你可以下载二进制文件并直接运行它，或者通过 Docker 启动它。通过 Docker 运行是测试它的最快方式。

```
docker run -it --rm -p 7700:7700 getmeili/meilisearch:latest
```

一旦服务器启动，你可以通过 HTTP 与它通信。最简单的用例是将文档索引到索引中并查询它们。以下是使用 Python 的示例：

```
import requests

docs = [
    {"id": 1, "title": "AI in Finance", "text": "How AI is changing banks"},
    {"id": 2, "title": "AI in Law", "text": "How AI helps legal teams"},
]
requests.put(
    "http://localhost:7700/indexes/articles/documents",
    json=docs
)
```

搜索同样简便。

```
import requests

query = "ai banks"
res = requests.post(
    "http://localhost:7700/indexes/articles/search",
    json={"q": query}
)
print(res.json())
```

引擎在毫秒内返回结果。容错、词语接近度和相关性排名开箱即用。

如果你配置了同义词，Meilisearch 会自动处理它们，并允许对价格或日期等属性进行自定义排序规则。它还支持分面、过滤和地理搜索，这使其适用于电子商务、旅行应用程序、房产列表和数据密集型仪表板。

## 结合使用混合搜索和 AI

混合搜索结合了全文搜索和语义向量搜索。

Meilisearch 通过向量字段支持语义搜索，并能够结合这两种方法。这帮助你为输入模糊查询或自然语言问题的用户提供服务。

AI 模型提供[嵌入][11]来捕捉意义，而 Meilisearch 负责快速检索和排名。

要添加语义搜索，首先需要使用 AI 模型为文档生成嵌入。以下是使用 OpenAI 嵌入的简单示例：

```
from openai import OpenAI
import requests

client = OpenAI()
def embed(text):
    out = client.embeddings.create(
        model="text-embedding-3-small",
        input=text
    )
    return out.data[0].embedding
doc = {
    "id": 3,
    "title": "AI in Insurance",
    "text": "How AI powers underwriting",
    "vector": embed("How AI powers underwriting")
}
requests.put(
    "http://localhost:7700/indexes/articles/documents",
    json=[doc]
)
```

当用户进行搜索时，你可以对查询进行嵌入并计算相似度。你可以将其与 Meilisearch 返回的关键字搜索结果进行混合。组合的排名创造了比单一方法更好的体验。

用户经常输入不完整或无结构的查询。他们可能会输入“AI如何帮助银行？”而不是关键词。

你可以使用AI模型将其重写为更易于搜索的内容。重写后的查询在Meilisearch中生成更好的结果，同时仍能尊重用户的原始意图。

```
from openai import OpenAI
import requests

client = OpenAI()
def rewrite_query(user_query):
    prompt = f"Rewrite this for search: {user_query}"
    out = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[{"role": "user", "content": prompt}]
    )
    return out.choices[0].message.content
user_query = "how does ai help banks"
rewritten = rewrite_query(user_query)
res = requests.post(
    "http://localhost:7700/indexes/articles/search",
    json={"q": rewritten}
)
print(res.json())
```

此模式提高了博客、文档平台和知识库中基于问题的搜索的准确性。你还可以使用该模型来规范化查询，删除歧义文本，扩展同义词，并在查询到达Meilisearch之前修正拼写错误。

## 使用Sevalla部署Meilisearch到云端

你可以在任何地方部署Meilisearch。它可以运行在小型服务器、本地机器或容器内。

自托管为你提供完全的控制，通常是希望将敏感数据保留在内部的技术团队的首选。你可以选择任何云提供商，如AWS、DigitalOcean或其他来设置Meilisearch。但我将使用Sevalla。

[Sevalla][12] 是一个为开发者和开发团队设计的PaaS提供商，能够以最高效的方式不断交付功能和更新。它为你的项目提供应用托管、数据库、对象存储和静态网站托管。

我出于两个原因使用Sevalla：

- 每个平台都会为你创建云资源收费。Sevalla为我们提供了50美元的信用额度，因此在这个例子中我们不会产生任何费用。
- Sevalla有一个[用于Melisearch的模板][13]，因此简化了每个你将需要的资源的手动安装和设置。

[登录][14]到Sevalla并点击Templates。你可以看到Melisearch作为模板之一。点击Deploy。

![Sevalla Template](https://cdn.hashnode.com/res/hashnode/image/upload/v1764079735090/db59a275-f641-4f3a-bae9-c1cab1360cbd.png)

你将看到为应用程序提供的资源。

![Sevalla Deployment](https://cdn.hashnode.com/res/hashnode/image/upload/v1764079768294/8ca6ccbc-2b64-4a3d-984e-f4cd17e19289.png)

部署完成后，点击“Visit app”。

![Melisearch in Production](https://cdn.hashnode.com/res/hashnode/image/upload/v1764079831010/98be4bbb-6e85-4572-a84e-d11114d1215d.png)

现在你有一个运行在云端的生产级Melisearch服务器。你可以用它为数据库设置索引，并使用JavaScript或其他SDK与Melisearch交互。

这是Melisearch提供的[完整API列表][15]。

## 结论

Meilisearch为你提供快速而优雅的搜索基础。AI模型增加了理解力和个性化。当这两者结合时，你会得到一种即时、自适应和智能的搜索体验。

你可以从简单的关键词搜索开始，然后添加查询重写、嵌入、混合搜索和重新排序。你还可以使用建议、同义词和过滤器来改善用户体验。

凭借其简单的API、广泛的语言支持和强大的生态系统，Meilisearch让你很容易在任何现代应用中构建感觉如鱼得水的搜索。

_希望你喜欢这篇文章。可以在_ [_Linkedin_][16] _找到我或_ [_访问我的网站_][17]_。_

[1]: https://github.com/meilisearch/meilisearch
[2]: https://www.turingtalks.ai/p/how-to-perform-sentence-similarity-check-using-sentence-transformers
[3]: #heading-understanding-meilisearch
[4]: #heading-setting-up-meilisearch
[5]: #heading-using-hybrid-search-and-ai-together
[6]: #heading-using-ai-to-rewrite-queries
[7]: #heading-deploying-meilisearch-to-the-cloud-using-sevalla
[8]: #heading-conclusion
[9]: https://saas.meilisearch.com/deals
[10]: https://aws.amazon.com/what-is/database-sharding/
[11]: https://www.ibm.com/think/topics/embedding
[12]: https://sevalla.com/
[13]: https://docs.sevalla.com/templates/overview
[14]: https://app.sevalla.com/login
[15]: https://www.meilisearch.com/docs/reference/api/overview
[16]: https://linkedin.com/in/manishmshiva
[17]: https://manishshivanandhan.com/

