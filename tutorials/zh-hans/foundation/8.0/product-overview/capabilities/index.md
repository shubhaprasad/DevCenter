---
layout: tutorial
title: 产品主要功能
weight: 1
---
<!-- NLS_CHARSET=UTF-8 -->
## 概述
{: #overview }
通过 {{ site.data.keys.product_full }}，您可以使用诸如开发、测试、后端连接、推送通知、脱机方式、更新、安全性、分析、监控和应用程序发布等功能。

### 开发
{: #deployment }
{{ site.data.keys.product }} 提供一个框架以支持安全移动应用程序（应用）的开发、优化、集成和管理。{{ site.data.keys.product }} 未介绍用户必须了解的专用编程语言或模型。


您可以使用 HTML5、CSS3 和 JavaScript 来开发应用程序。可以选择编写本机代码（Java 或 Objective-C）。{{ site.data.keys.product }} 提供了一个 SDK，其中包含可通过本机代码访问的库。

#### 受支持的平台
{: #supported-platforms }
{{ site.data.keys.product }} SDK 支持以下平台：

* iOS
* Android
* Windows Universal 8.1 和 Windows 10 UWP
* Web 应用程序

### 后端连接
{: #back-end-connections }
某些移动应用程序会在完全脱机状态下运行，不连接到任何后端系统，但大部分移动应用程序会连接到现有企业服务，以提供与用户相关的关键功能。例如，用户可随时随地使用移动应用程序购物，而不受商店的营业时间限制。这些订单仍要使用商店的现有电子商务平台进行处理。要将移动应用程序与企业服务进行集成，必须使用中间件，如移动网关。{{ site.data.keys.product }} 可以充当此中间件解决方案，并使与后端服务的通信更轻松。

### 推送通知
{: #push-notifications }
利用推送通知，企业应用程序可以将信息发送给移动设备，即使未使用此应用程序也如此。{{ site.data.keys.product }} 包含一个统一的通知框架，用于为此类推送通知提供一致的机制。通过该统一的通知框架，您可以发送推送通知，而无需知道每个目标设备或平台的详细信息，因为针对推送通知，每个移动平台都有不同的机制。

### 脱机方式
{: #offline-mode }
在连接方面，移动应用程序能够以脱机、联机或混合方式运行。{{ site.data.keys.product }} 使用客户机/服务器体系结构，该体系结构可以检测设备是否有网络连接以及连接的质量。充当客户机时，移动应用程序会定期尝试连接到服务器，并评估连接强度。在移动设备缺乏连接时，可以使用支持脱机的移动应用程序，但部分功能会受到限制。在创建支持脱机的移动应用程序时，存储有关此移动设备的信息很有用，这有助于在脱机方式下保持其功能。此信息通常来自后端系统，在应用程序体系结构中，您必须考虑与后端保持数据同步。{{ site.data.keys.product }} 包含用于数据交换和存储的名为 JSONStore 的功能。通过此功能，您可以从数据源创建、读取、更新和删除数据记录。脱机运行时，每项操作都会排队。在连接可用时，操作会转移到服务器，随后会针对源数据执行每项操作。

### 更新
{: #update }
{{ site.data.keys.product }} 简化了版本管理和移动应用程序兼容性。每次用户启动移动应用程序时，应用程序都会与服务器进行通信。
通过使用该服务器，{{ site.data.keys.product }} 可以确定是否有较新的应用程序版本可用，如果有，则向用户提供有关该版本的信息，或将应用程序更新推送到设备。服务器还可强制升级到最新版本的应用程序，以防止继续使用过时的版本。

### 安全性
{: #security }
保护保密信息和私有信息对企业内的所有应用程序而言都十分重要，这包括移动应用程序。可在多种级别上应用移动安全性，如移动应用程序、移动应用程序服务或后端服务。必须保护客户隐私，使保密数据免遭未授权用户的访问。处理私有移动设备即意味着放弃控制诸如移动操作系统之类的某些较底层的安全性。

{{ site.data.keys.product }} 通过配置监控移动应用程序与后端系统之间数据流的服务器来提供安全的端到端通信。通过 {{ site.data.keys.product }}，您可以为此数据流的任何访问定义定制安全处理程序。由于移动应用程序数据的任意访问都必须经过此服务器实例，因此可以为移动应用程序、Web 应用程序和后端访问定义不同的安全处理程序。通过这种细化的安全性，您可以为移动应用程序的不同功能定义不同的认证级别。您还可以防止移动应用程序访问敏感信息。

### 分析
{: #analytics }
{{ site.data.keys.mf_analytics }}功能支持在应用程序、服务、设备和其他源中进行搜索，以收集有关使用情况的数据，或检测问题。

除了总结应用程序活动的报告，{{ site.data.keys.product }} 还包含一个可在 {{ site.data.keys.mf_console }} 中访问的可扩展操作分析平台。
{{ site.data.keys.mf_analytics_short }}功能支持企业在从设备、应用程序和服务器收集的日志和事件中搜索模式、问题和平台使用情况统计信息。您可以根据需求来启用分析和/或报告。

### 监控
{: #monitoring }
{{ site.data.keys.product }} 包含一系列操作分析和报告机制，用于收集、查看和分析 {{ site.data.keys.product }} 应用程序及服务器中的数据，以及监控服务器运行状况。

### 应用程序发布
{: #application-publishing }
{{ site.data.keys.product }} Application Center 是企业应用程序存储器。利用 Application Center，您可以安装、配置和管理移动应用程序存储库，以供企业内的个人和小组使用。您可以控制在贵组织内谁可以访问 Application Center 并将应用程序上载到 Application Center 存储库，以及谁可以将这些应用程序下载到移动设备并进行安装。您还可使用 Application Center 来收集用户的反馈，并访问有关安装了应用程序的设备的信息。

Application Center 的概念类似于 Apple 公共 App Store 或 Google Play 商店的概念，只是它是以开发流程为目标的。

Application Center 提供一个用于存储移动应用程序文件的存储库和一个用于管理该存储库的基于 Web 的控制台。Application Center 还提供移动客户机应用程序，以允许用户浏览由 Application Center 存储的应用程序的目录，安装应用程序，为开发团队提供反馈以及向 IBM Endpoint Manager 公开生产应用程序。使用访问控制表 (ACL)，可控制对通过 Application Center 下载和安装应用程序操作的访问权限。