--- 
title: 如何通过 Terraform 和 GitOps 简化 AWS 多账户管理 
date: 2024-12-09T08:13:52.525Z 
author: Nitheesh Poojary 
authorURL: https://www.freecodecamp.org/news/author/nitheeshp/ 
originalURL: https://www.freecodecamp.org/news/simplify-aws-multi-account-management-with-terraform-and-gitops/ 
posteditor: "miyaliu" 
proofreader: "" 
--- 
 
过去，在云计算的世界里，一家公司的旅程通常从一个 AWS 账户开始。在这个统一的空间里，开发和测试环境共存，而生产环境则存在于一个独立的账户中。
 
<!-- more --> 
 
这种安排在早期可能运作良好，但随着公司成长及需求变得更加专业化，单个账户的简单性可能开始暴露其局限性。对专用环境的需求将开始增加，不久后，该公司可能需要为诸如安全、DevOps 和计费等特定功能创建新的 AWS 账户。 
 
每增加一个新账户，整个基础设施的安全策略和日志管理的复杂性就会呈指数级增长。公司的云架构师们会意识到他们需要一种更加集中和精简的方法来管理这一扩展的数字存在。
 
### 引入 AWS Organizations 
 
AWS Organizations 是一项旨在简化 AWS 账户管理的服务。这个强大的工具允许您将多个 AWS 账户组合在一个统一的架构下。通过 AWS Organizations，您可以轻松创建组织单元，应用服务控制策略，并管理所有账户的权限。这不仅简化了流程，还增强了安全性和合规性。 
 
通过支付的集中处理以及为每个账户生成详尽的费用报告，AWS Organizations 的计费流程也得到了优化。这种改进的财务管理清晰度使公司能够更高效地分配资源，并为未来的扩展制定战略。 
 
AWS Organizations 可以帮助您的团队持续执行安全策略，在所有账户中实现日志记录，并简化管理任务。云基础设施现在已成为一个井然有序、安全高效的机器，准备支持公司未来多年的雄心壮志。 
 
在本文中，我们将讨论多账户设置的意义及其工作原理。我将带您了解从部署架构到创建组织单元等所有方面。 
 
## 目录 
 
-   [多账户设置的组件][1] 
     
-   [如何自动化多账户策略][2] 
     
-   [AWS 组织结构][3] 
     
-   [部署架构][4] 
     
-   [CI/CD 组件概述][5] 
     
-   [CI/CD 部署过程说明][6] 
     
-   [如何自动化创建登陆区][7] 
     
-   [如何创建组织单元][8] 
     
-   [如何自动化将控制塔控件附加到 OU][9] 
     
-   [结论][10] 
     
 
## **多账户设置的组件** 
 
首先，让我们详细看看构成 AWS 多账户策略的各种组件： 
 
-   **AWS 控制塔** 
     
-   **登陆区** 
     
-   **AWS 组织单元 (OU)** 
     
-   **AWS 单点登录 (SSO)** 
     
-   **控制塔控件** 
     
-   **服务控制策略 (SCPs)** 
     
 
### **什么是 AWS 控制塔？** 
 
AWS 控制塔是一项全面的服务，使您能够高效地设置和管理多账户 AWS 环境。它是基于 AWS 专家从最佳实践制定的，并遵循行业标准和要求。 
 
通过使用 AWS 控制塔，您可以确保您的 AWS 环境是安全、合规且井井有条的，从而促进更容易的管理和可扩展性。 
 
#### AWS 控制塔的功能： 
 
-   云 IT 可以确保所有账户均符合公司范围的规定，并且分布式团队可以快速创建新的 AWS 账户。 
     
-   您可以通过预配置的控件执行最佳实践、标准和监管要求。 
     
-   您可以使用最佳实践蓝图自动设置 AWS 环境。这些蓝图涵盖各种方面，例如多账户结构、身份和访问管理以及账户配置工作流。 
     
-   它允许您管理新建或现有的账户配置，了解合规状态，并大规模执行控件。 
     
 
### **AWS 中的登陆区是什么？** 
 
登陆区通过自动化帮助您快速设置云环境，包括遵循行业最佳实践的预配置设置，以确保您的 AWS 账户安全。 
 
起点为公司高效启动和实施工作负载及应用程序提供了一种基础，确保了一个安全可靠的基础设施环境。 
 
创建登陆区有两种选择。第一，您可以使用 AWS 控制塔仪表板。第二，您可以构建一个自定义登陆区。如果您是 AWS 的新手，我建议使用 AWS 控制塔来创建登陆区。 
 
![AWS 登陆区](https://cdn.hashnode.com/res/hashnode/image/upload/v1732137800622/f72dbf02-fa34-4004-999d-71a2af33f90b.png) 
 
-   多账户环境与 AWS 组织。 
     
-   通过 AWS IAM 身份中心的默认目录进行身份管理。 
     
-   使用 IAM 身份中心对账户进行联合访问。 
     
-   从 AWS CloudTrail 和 AWS Config 集中记录日志并存储在 Amazon Simple Storage Service (Amazon S3) 中。 
     
-   启用跨账户[安全审计][11]，使用 IAM 身份中心。 
     
 
### **什么是 AWS 组织单位？** 
 
使用多个账户可以更好地支持您的安全目标和公司运营。 
 
AWS 组织允许基于策略管理多个 AWS 账户。当您创建新账户时，可以将它们安排在组织单位（OU）中，这些组织单位是提供相同应用程序或服务的账户组合。 
 
![AWS 组织单位](https://cdn.hashnode.com/res/hashnode/image/upload/v1732137833615/6eebb3ab-94d0-4286-8dc4-9d3ae297e186.png) 
 
#### 使用 OU 的优势： 
 
-   账户是安全保护的单元。潜在的危险和安全威胁可以被限制在一个账户中，而不影响其他账户。 
     
-   团队有不同的任务和资源需求。设置不同的账户可防止团队之间的干扰，就像他们使用相同账户时可能发生的情况。 
     
-   将数据存储隔离到一个账户中可以减少有权访问和管理数据存储的人员数量。 
     
-   多账户概念允许您为业务部门、功能团队或个人用户生成单独的可计收费项目。 
     
-   AWS 配额是根据账户设置的。将工作负载分离到不同的账户中，使每个账户都拥有单独的配额。 
     
 
### **什么是 AWS IAM 身份中心？** 
 
AWS IAM 身份中心为管理多个 AWS 账户和业务应用程序的访问提供了集中化解决方案。 
 
![AWS 身份中心](https://cdn.hashnode.com/res/hashnode/image/upload/v1732137875918/349673f8-1a09-4bcc-b1db-6a898d3d06b5.png) 
 
此方法提供单点登录功能，让员工可以使用单一凭证访问所有指定的账户和应用程序。 
 
个性化的网络用户门户为用户提供了在 AWS 账户中分配角色的集中视图。 
 
为了统一的认证体验，用户可以使用 AWS 命令行界面、AWS SDK 或 AWS 控制台移动应用通过他们的目录凭据登录。 
 
您还可以在 IAM 身份中心的身份存储中设置和管理用户 ID，或者连接到您现有的身份提供商，如 Microsoft Active Directory、Okta 等。 
 
### **控制塔控制（护栏）** 
 
控件是关于安全、运营和合规的预定义治理规则。您可以选择并将它们应用于整个企业或特定账户组。 
 
![ControlTowerControls](https://cdn.hashnode.com/res/hashnode/image/upload/v1732137911519/5dac3db6-15e6-476b-9b50-a1597a02fe84.png) 
 
控件可以是侦测性、预防性或主动性的，可以是强制性的或可选的。 
 
-   首先，我们有侦测性控件（例如，检测是否允许对 Amazon S3 存储桶进行公共读取访问）。 
     
-   其次，预防性控件设立意图和防止不符合您策略的资源部署（例如，在所有账户中启用 AWS CloudTrail）。 
     
-   最后，主动控制能力使用 [AWS CloudFormation 钩子][12] 主动识别并阻止不符合您启用控件的资源的 CloudFormation 部署。例如，开发人员不能创建可以在休眠状态下存储未加密数据的 S3 存储桶。 
     
 
### **服务控制策略（SCP）** 
 
SCP 是组织的一项功能，允许您为组织内的成员账户设定最大权限。 
 
![服务控制策略](https://cdn.hashnode.com/res/hashnode/image/upload/v1732137972306/80d0782c-0801-4548-9c0c-d4a11d43ecbe.png) 
 
SCP 有许多功能和特性： 
 
-   如果 SCP 拒绝了一个账户上的操作，则即便账户中的实体有 IAM 权限，也无法执行该操作。 
     
-   防止停止或删除 CloudTrail 日志。 
     
-   防止删除 VPC 流日志。 
     
-   禁止 AWS 账户离开组织。 
     
-   防止 AWS GuardDuty 变更。 
     
-   防止使用 AWS 资源访问管理器（RAM）进行外部或跨环境的资源共享。 
     
-   防止禁用默认的 Amazon EBS 加密。 
     
-   防止上传未加密的 Amazon S3 对象。 
     
-   并防止受影响账户中的 IAM 用户和角色创建请求中未包含指定标签的某些资源类型。 
     
 
## **如何自动化多账户策略** 
 
现在，您已经了解了 AWS 中多账户策略的关键概念，让我们深入探讨实际部分。 
 
在接下来的小节中，我们将介绍如何设置 AWS 控制塔、创建着陆区，并自动创建组织单位（OU）。我还会引导您配置控制塔控件，通常称为护栏，以在您的 AWS 环境中维护安全性、合规性和治理。 
 
-   创建一个名为 Core 的 AWS Organizations OU（组织单元），位于组织的根结构中。 
 
-   创建并添加两个共享账户到 Security OU：日志归档账户和审核账户。 
 
-   在 IAM Identity Center 中创建一个云原生目录，具备现成的组和单点登录访问功能。 
 
-   应用所有必要的预防性控制来强制执行策略。 
 
-   应用必要的检测性控制以识别配置违规。 
 
## **AWS 组织结构** 
 
我们将创建并实施以下的组织结构。您可以根据需要添加或修改 OU。 
 
![AWS 组织结构](https://cdn.hashnode.com/res/hashnode/image/upload/v1732138006995/423e54cd-bf74-4aef-a2a3-d52294482ca0.png) 
 
## **部署架构** 
 
我将使用 Terraform Cloud 和 GitHub Actions 来自动化整个过程。此架构适用于所有三个组件，包括核心账户、着陆区以及组织单元（OU）的创建和控制。 
 
![部署架构](https://cdn.hashnode.com/res/hashnode/image/upload/v1732138041912/0cba5af0-69ea-4ae9-986e-c1608d3d5c21.avif) 
 
### CI/CD 组件概述 
 
#### **1\. GitHub Actions** 
 
GitHub Actions 是一个 CI/CD 平台，可让您自动化构建、测试和部署管道。您可以创建自动构建和测试每个拉取请求的工作流，以确保代码更改在合并之前得到验证。 
 
GitHub Actions 还允许您将合并后的拉取请求部署到生产环境，简化发布流程并减少错误。 
 
使用 GitHub Actions 可以增强您的开发工作流，提高代码质量，加快新功能和更新的交付速度。 
 
#### **2\. Terraform Cloud** 
 
Terraform Cloud 是 HashiCorp 提供的用于管理和执行 Terraform 代码的平台。它提供增强开发人员和 DevOps 工程师之间协作的工具和功能，使团队合作更高效。 
 
通过 Terraform Cloud，您可以简化和流线化您的工作流，使处理复杂的基础设施任务和部署变得更容易。该平台还提供强大的安全功能来保护您的代码和基础设施，确保您的产品在整个生命周期中的安全性。 
 
### **CI/CD 部署过程解释** 
 
DevOps 工程师负责编写 Terraform 代码，然后创建一个拉取请求。我在 `terraform-plan.yml` 文件中添加了几个 Terraform 代码的测试用例，这些用例只在特性分支上运行。 
 
-   **检查环境变量：** 确保所有必要的环境变量已设置。 
 
-   **检出代码：** 使用 `actions/checkout` 操作检出存储库。 
 
-   **验证检出：** 验证检出操作是否成功。 
 
-   **验证：** 验证 Terraform 代码是否存在任何语法错误。拉取请求包含代码的建议更改，允许团队成员查看并将其合并到主分支中。一旦拉取请求与主分支合并，所有测试用例将重新运行，并通过 Terraform Cloud 创建着陆区。 
 
## **在设置控制塔之前需要了解的事项** 
 
在开始设置 AWS Control Tower 的过程之前，重要的是要清楚了解 Control Tower 的限制，并考虑一些关键点。 
 
- 在设置着陆区时，重要的是要选择您的主区域。一旦您做出选择，您将无法更改主区域。 
 
- 如果您打算在已属于现有组织单元（OU）的现有 AWS 账户上建立控制塔，则无法使用它。为了继续，您需要创建一个不与任何组织单元（OU）关联的新 AWS 账户。 
 
- 作为 Control Tower 创建过程的一部分，您需要创建如日志归档账户和审核账户等必需账户。需要账户特定的电子邮件。 
 
- 为了在管理账户中设置着陆区，非常重要的是要确保您已在管理账户中订阅以下服务： 
 
    -   S3, EC2, SNS, VPC, CloudFormation, CloudTrail, CloudWatch, AWS Config, IAM, AWS Lambda 
 
-   AWS Control Tower 基线仅涵盖少数服务，且自定义选项有限：IAM Identity Center、CloudTrail、Config、一些配置规则和一些 AWS Organizations 中的 SCP。 
 
-   实施 IAM Identity Center 的限制仅限于组织的管理账户。 
 
-   AWS Control Tower 实现并发限制，只允许一次执行一个操作。 
 
-   请注意，某些 AWS 区域不支持在 AWS Control Tower 中操作某些控制。这是因为指定的区域缺乏支持所需操作的必要底层功能。 
 
### **如何创建控制塔** 
 
创建控制塔意味着设置一个着陆区。AWS 着陆区需要创建两个新的成员账户：审核账户和日志归档账户。您将需要这两个账户的两个唯一电子邮件地址。 
 
## **如何自动化创建登陆区域** 
 
当你运行这段代码时，将在核心 OU 下创建核心 OU 和两个账户。我为每个组件提到了两个代码库：一个用于部署 AWS 资源，如登陆区域、OU 和控制塔控制，另一个用于 Terraform 模块。 
 
_Terraform 模块_是一组位于特定目录中的标准配置文件。Terraform 模块将某一特定任务的资源进行分组，从而减少了相似基础设施组件所需的代码量。 
 
我将核心账户创建和登陆区域创建模块都导入到了同一个 [`main.tf`][13] 文件中。这是必要的，因为登陆区域的创建依赖于核心账户模块。将它们一起包含能够确保所有的依赖关系得到良好管理，且使得部署过程更加高效。 
 
这种方法还简化了项目结构，有助于避免在单独管理这些组件时可能会出现的问题。 
 
AWS Control Tower 的 [`CreateLandingZone`][14] API 需要一个登陆区域版本和一个清单文件作为输入参数。以下是一个 **LandingZoneManifest.json** 清单的示例。 
 
``` 
{ 
   "governedRegions": ["us-west-2","us-west-1"], 
   "organizationStructure": { 
       "security": { 
           "name": "CORE" 
       }, 
       "sandbox": { 
           "name": "Sandbox" 
       } 
   }, 
   "centralizedLogging": { 
        "accountId": "222222222222", 
        "configurations": { 
            "loggingBucket": { 
                "retentionDays": 60 
            }, 
            "accessLoggingBucket": { 
                "retentionDays": 60 
            }, 
            "kmsKeyArn": "arn:aws:kms:us-west-1:123456789123:key/e84XXXXX-6bXX-49XX-9eXX-ecfXXXXXXXXX" 
        }, 
        "enabled": true 
   }, 
   "securityRoles": { 
        "accountId": "333333333333" 
   }, 
   "accessManagement": { 
        "enabled": true 
   } 
} 
``` 
 
该模块使用 `landingzone_manifest_template` 来设置 AWS 登陆区域。登陆区域版本和管理员账户 ID 通过变量提供。此模块还创建了设置登陆区域所需的多个 IAM 角色。 
 
我定义了一个本地变量 `landingzone_manifest_template`，它是用于设置登陆区域的 JSON 模板。此 JSON 模板包含几个重要设置： 
 
``` 
provider "aws" { 
  region = var.region 
} 
 
locals { 
  landingzone_manifest_template = <<EOF 
{ 
    "governedRegions": ${jsonencode(var.governed_regions)}, 
    "organizationStructure": { 
        "security": { 
            "name": "Core" 
        } 
    }, 
    "centralizedLogging": { 
         "accountId": "${module.aws_core_accounts.log_account_id}", 
         "configurations": { 
             "loggingBucket": { 
                 "retentionDays": ${var.retention_days} 
             }, 
             "accessLoggingBucket": { 
                 "retentionDays": ${var.retention_days} 
             } 
         }, 
         "enabled": true 
    }, 
    "securityRoles": { 
         "accountId": "${module.aws_core_accounts.security_account_id}" 
    }, 
    "accessManagement": { 
         "enabled": true 
    } 
} 
EOF 
} 
 
module "aws_core_accounts" { 
  source = "https://github.com/nitheeshp-irl/terraform_modules/aws_core_accounts_module" 
 
  logging_account_email  = var.logging_account_email 
  logging_account_name   = var.logging_account_name 
  security_account_email = var.security_account_email 
  security_account_name  = var.security_account_name 
} 
 
module "aws_landingzone" { 
  source                  = "https://github.com/nitheeshp-irl/blog_terraform_modules/aws_landingzone_module" 
  manifest_json           = local.landingzone_manifest_template 
  landingzone_version     = var.landingzone_version 
  administrator_account_id = var.administrator_account_id 
} 
``` 
 
-   **治理区域**: 指定登陆区域所治理的区域。 
     
-   **组织结构**: 定义安全结构，包含一个专用的安全账户。 
     
-   **集中式日志记录**: 配置日志记录，指定账户 ID 和日志的保留策略。 
     
-   **安全角色**: 指定安全角色的账户 ID。 
     
-   **访问管理**: 启用访问管理。 
     
-   **核心账户**: 核心账户代码也在同一文件中定义，它设置了日志和安全所需的基本 AWS 账户。 
     
 
你可以在这里找到完整代码：[https://github.com/nitheeshp-irl/aws-landing-zone][15]。 
 
## **如何创建组织单位** 
 
当你运行这段代码时，将根据 [变量][16] 文件中的规范创建不同的组织单位（OUs）。 
 
一旦登陆区域设置完成，我们可以根据业务需求创建 OU。这将从变量文件中获取 OU 名称并创建 OU。 
 
``` 
aws_region = "us-east-2" 
 
organizational_units = [ 
  { 
    unit_name = "apps" 
  }, 
  { 
    unit_name = "infra" 
  }, 
  { 
    unit_name = "stagingpolicy" 
  }, 
  { 
    unit_name = "sandbox" 
  }, 
  { 
    unit_name = "security" 
  } 
] 
``` 
 
你可以在以下位置查看代码： 
 
-   [AWS 组织单位（OUs）Terraform 仓库][17] 
     
-   [AWS 组织单位 Terraform 模块路径][18] 
     
 
一旦你使用上述的库创建了 OU 单位，本仓库将应用 Control Tower 控制到这些 OU。 
 
创建所需对象后，如果需要，可以将控制附加到 OU。以下是 [`main.tf`][19] 文件： 
 
``` 
provider "aws" { 
  region = var.region 
} 
 
module "aws_controls" { 
  source = "https://github.com/nitheeshp-irl/blog_terraform_modules/awscontroltower-controls_module" 
 
  aws_region = var.aws_region 
  controls   = var.controls 
} 
``` 
 
我们使用 Terraform 模块来创建 AWS 资源。 
 
以下是控制变量： 
 
``` 
aws_region = "us-east-2" 
 
 
controls = [ 
  { 
    control_names = [ 
      "AWS-GR_ENCRYPTED_VOLUMES", 
      "AWS-GR_EBS_OPTIMIZED_INSTANCE", 
      "AWS-GR_EC2_VOLUME_INUSE_CHECK", 
      "AWS-GR_RDS_INSTANCE_PUBLIC_ACCESS_CHECK", 
      "AWS-GR_RDS_SNAPSHOTS_PUBLIC_PROHIBITED", 
      "AWS-GR_RDS_STORAGE_ENCRYPTED", 
      "AWS-GR_RESTRICTED_COMMON_PORTS", 
      "AWS-GR_RESTRICTED_SSH", 
      "AWS-GR_RESTRICT_ROOT_USER", 
      "AWS-GR_RESTRICT_ROOT_USER_ACCESS_KEYS", 
      "AWS-GR_ROOT_ACCOUNT_MFA_ENABLED", 
      "AWS-GR_S3_BUCKET_PUBLIC_READ_PROHIBITED", 
      "AWS-GR_S3_BUCKET_PUBLIC_WRITE_PROHIBITED", 
    ], 
    organizational_unit_names = ["infra", "apps"] 
  } 
] 
``` 
 
你可以在这里看到代码： 
 
-   [创建控制塔控制的 Terraform 仓库][20] 
     
-   [用于创建控制塔控制的 Terraform 模块][21] 
     
 
## **结论** 
 
在 AWS 中跨账户策略的导航可能具有挑战性，但通过 AWS 控制塔和结构化的方法，它变得可管理。 
 
使用 AWS 控制塔，你的团队可以确保他们的 AWS 环境是安全的、合规的并且组织良好。通过 AWS 组织进行的自动化设置、可伸缩的治理和集中管理为云基础设施提供了良好的基础。 
 
通过 AWS 控制塔实施一个登陆区提供了一个安全和标准化的起点，允许更快的部署和更好的治理。使用组织单位 (OUs) 根据业务需求对账户进行细分，提高了安全性和运营效率。AWS IAM 身份中心简化了访问管理，提供了跨多个账户和应用的统一身份验证体验。 
 
服务控制策略 (SCPs) 通过确保所有资源都遵循组织的规则来帮助保持安全和合规性。Terraform Cloud 和 GitHub Actions 使得资源部署变得更加容易，提供了一个顺畅的 CI/CD 管道来管理基础设施变更。 
 
[1]: #heading-components-of-multi-account-setup 
[2]: #heading-how-to-automate-a-multi-account-strategy 
[3]: #heading-aws-organization-structure 
[4]: #heading-deployment-architecture 
[5]: #heading-overview-of-cicd-components 
[6]: #heading-cicd-deployment-process-explained 
[7]: #heading-how-to-automate-landing-zone-creation 
[8]: #heading-how-to-create-an-organizational-unit 
[9]: #heading-how-to-automate-attaching-control-tower-control-to-the-ou 
[10]: #heading-conclusion 
[11]: https://docs.aws.amazon.com/general/latest/gr/aws-security-audit-guide.html 
[12]: https://aws.amazon.com/blogs/mt/proactively-keep-resources-secure-and-compliant-with-aws-cloudformation-hooks/ 
[13]: https://github.com/nitheeshp-irl/aws-landing-zone/blob/main/main.tf 
[14]: https://docs.aws.amazon.com/controltower/latest/APIReference/API_CreateLandingZone.html 
[15]: https://github.com/nitheeshp-irl/aws-landing-zone 
[16]: https://github.com/nitheeshp-irl/aws-orgunits/blob/main/variables.auto.tfvars 
[17]: https://github.com/nitheeshp-irl/aws-orgunits 
[18]: https://github.com/nitheeshp-irl/blog_terraform_modules/tree/main/aws_org_module 
[19]: https://github.com/nitheeshp-irl/controltower_controls/blob/main/main.tf 
[20]: https://github.com/nitheeshp-irl/controltower_controls 
[21]: https://github.com/nitheeshp-irl/blog_terraform_modules/tree/main/awscontroltower-controls_module 
 
 
