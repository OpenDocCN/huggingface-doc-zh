# 门控数据集

> 原始文本：[https://huggingface.co/docs/hub/datasets-gated](https://huggingface.co/docs/hub/datasets-gated)

为了更好地控制数据集的使用方式，Hub允许数据集作者为其数据集启用**访问请求**。用户必须同意与数据集作者共享其联系信息（用户名和电子邮件地址）以访问数据集文件。数据集作者可以配置此请求以包含额外字段。启用访问请求的数据集称为**门控数据集**。访问请求始终授予个人用户，而不是整个组织。门控数据集的常见用例是在更广泛发布之前提供对早期研究数据集的访问。

## 作为数据集作者管理门控数据集

要启用访问请求，请转到数据集设置页面。默认情况下，数据集未被门控。单击右上角的**启用访问请求**。

![](../Images/8b51f89329266b62fd72bdb72d9395fd.png) ![](../Images/120fbd70f1a99e7a7961cec6f977e549.png)

默认情况下，用户请求访问数据集时将自动授予对数据集的访问权限。这称为**自动批准**。在此模式下，用户只要与您分享了个人信息，就可以访问您的数据集。

![](../Images/fecc327e50429c1862a85875b44cb27f.png) ![](../Images/416dd49a6e8e6459a07d654c70637b26.png)

如果您想手动批准哪些用户可以访问您的数据集，您必须将其设置为**手动批准**。在这种情况下，您将注意到更多选项：

+   **添加访问**允许您搜索用户并授予他们访问权限，即使他们没有请求。

+   **通知频率**允许您配置何时在新用户请求访问时收到通知。它可以设置为每天一次或实时。默认情况下，电子邮件将发送到您的主要电子邮件地址。您可以在**通知电子邮件**字段中设置不同的电子邮件地址。对于托管在组织下的数据集，电子邮件将发送给组织的前5个管理员。

![](../Images/453673d7f66c9c5a2cabb301b382db79.png) ![](../Images/b07df5dc3e8bb8c167aed6870c4690e5.png)

### 审查访问请求

启用访问请求后，您可以完全控制谁可以访问您的数据集，无论批准模式是手动还是自动。您可以通过UI或通过API审查和管理请求。

### 通过UI

您可以通过单击**审查访问请求**按钮从其设置页面查看谁可以访问您的门控数据集。这将打开一个带有3个用户列表的模态框：

+   **待处理**：等待批准访问您的数据集的用户列表。除非您选择了**手动批准**，否则此列表为空。您可以选择**接受**或**拒绝**请求。如果请求被拒绝，用户将无法访问您的数据集，也无法再次请求访问。

+   **已接受**：具有访问权限的用户的完整列表。您可以随时选择**拒绝**任何用户的访问权限，无论批准模式是手动还是自动。您还可以**取消**批准，这将将用户移至*待处理*列表。

+   **已拒绝**：您手动拒绝的用户列表。这些用户无法访问您的数据集。如果他们转到您的数据集存储库，他们将看到一条消息*您的访问此存储库的请求已被存储库的作者拒绝*。

![](../Images/681d1799a60d11eb0c9414b52ff458da.png) ![](../Images/12696c5879bd12585050822ee74ca791.png)

#### 通过API

您可以使用API自动批准访问请求。您必须传递一个具有对门控存储库的“写”访问权限的`token`。要生成一个令牌，请转到[您的用户设置](https://huggingface.co/settings/tokens)。

| 方法 | URI | 描述 | 标头 | 负载 |
| --- | --- | --- | --- | --- |
| `GET` | `/api/datasets/{repo_id}/user-access-request/pending` | 检索待处理请求列表。 | `{"authorization": "Bearer $token"}` |  |
| `GET` | `/api/datasets/{repo_id}/user-access-request/accepted` | 检索已接受请求的列表。 | `{"authorization": "Bearer $token"}` |  |
| `GET` | `/api/datasets/{repo_id}/user-access-request/rejected` | 检索已拒绝请求的列表。 | `{"authorization": "Bearer $token"}` |  |
| `POST` | `/api/datasets/{repo_id}/user-access-request/handle` | 更改给定访问请求的状态为`status`。 | `{"authorization": "Bearer $token"}` | `{"status": "accepted"/"rejected"/"pending", "user": "用户名"}` |
| `POST` | `/api/datasets/{repo_id}/user-access-request/grant` | 允许特定用户访问您的repo。 | `{"authorization": "Bearer $token"}` | `{"user": "用户名"}` |

上述HTTP端点的基本URL为`https://huggingface.co`。

**新功能!** 这些端点现在在我们的Python客户端`huggingface_hub`中得到官方支持。使用[`list_pending_access_requests`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.list_pending_access_requests)、[`list_accepted_access_requests`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.list_accepted_access_requests)和[`list_rejected_access_requests`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.list_rejected_access_requests)列出您数据集的访问请求。您还可以使用[`accept_access_request`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.accept_access_request)、[`cancel_access_request`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.cancel_access_request)、[`reject_access_request`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.reject_access_request)接受、取消和拒绝访问请求。最后，您可以使用[`grant_access`](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.grant_access)向用户授予访问权限。

### 下载访问报告

您可以使用**下载用户访问报告**按钮下载受限数据集的所有访问请求的报告。单击它以下载一个包含用户列表的json文件。对于每个条目，您有：

+   **用户**: 用户ID。示例：*julien-c*。

+   **全名**: 在Hub上的用户姓名。示例：*Julien Chaumond*。

+   **状态**: 请求的状态。可以是`"pending"`、`"accepted"`或`"rejected"`之一。

+   **电子邮件**: 用户的电子邮件。

+   **时间**: 用户最初发出请求的日期时间。

### 自定义请求的信息

默认情况下，访问您的受限数据集的用户将被要求通过单击**同意并发送请求以访问repo**按钮来分享他们的联系信息（电子邮件和用户名）。

![](../Images/0f7d25e8938d6652e3711c0dedf5cc0a.png) ![](../Images/0973311c0458cc641660edb0a07af6e9.png)

如果您想要请求更多用户信息以提供访问权限，您可以配置额外字段。这些信息将可以从**设置**选项卡中访问。为此，请向您的[数据集卡片元数据](./datasets-cards#dataset-card-metadata)添加一个`extra_gated_fields`属性，其中包含一组键/值对。*键*是字段的名称，*值*是其类型或带有`type`字段的对象。字段类型列表如下：

+   `文本`: 单行文本字段。

+   `复选框`: 复选框字段。

+   `date_picker`: 日期选择器字段。

+   `国家`: 一个国家下拉菜单。国家列表基于[ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)标准。

+   `选择`: 一个带有选项列表的下拉菜单。选项列表在`options`字段中定义。示例：`options: ["选项1", "选项2", {label: "选项3", value: "opt3"}]`。

最后，您还可以使用`extra_gated_prompt`额外字段来个性化向用户显示的消息。

这是一个定制请求表单的示例，用户被要求提供其公司名称和国家，并承认数据集仅供非商业用途。

```py
---
extra_gated_prompt: "You agree to not use the dataset to conduct experiments that cause harm to human subjects."
extra_gated_fields:
  Company: text
  Country: country
  Specific date: date_picker
  I want to use this dataset for:
    type: select
    options: 
      - Research
      - Education
      - label: Other
        value: other
  I agree to use this dataset for non-commercial use ONLY: checkbox
---
```

在某些情况下，您可能还希望修改门户标题中的文本和按钮中的文本。对于这些用例，您可以像这样修改`extra_gated_heading`和`extra_gated_button_content`：

```py
---
extra_gated_heading: "Acknowledge license to accept the repository"
extra_gated_button_content: "Acknowledge license"
---
```

## 作为用户访问受限数据集

作为用户，如果您想使用受限数据集，您将需要请求访问。这意味着您必须登录到Hugging Face用户帐户。

只能从浏览器中请求访问。转到Hub上的数据集，您将被提示共享您的信息：

![](../Images/0f7d25e8938d6652e3711c0dedf5cc0a.png) ![](../Images/0973311c0458cc641660edb0a07af6e9.png)

点击**同意**，即表示您同意与数据集作者共享您的用户名和电子邮件地址。在某些情况下，可能会要求提供额外的字段。为了帮助数据集作者决定是否授予您访问权限，请尽可能完整地填写表单。

一旦发送访问请求，有两种可能性。如果批准机制是自动的，您将立即获得对数据集文件的访问权限。否则，请求必须由作者手动批准，这可能需要更多时间。

数据集作者对数据集访问拥有完全控制权。特别是，他们可以随时决定在没有事先通知的情况下阻止您访问数据集，无论批准机制如何或者您的请求是否已经获得批准。

### 下载文件

要从受限数据集中下载文件，您需要进行身份验证。在浏览器中，只要您使用您的帐户登录，这是自动的。如果您使用脚本，您将需要提供一个[user token](./security-tokens)。在Hugging Face Python生态系统（`transformers`、`diffusers`、`datasets`等）中，您可以使用[`huggingface_hub`](https://huggingface.co/docs/huggingface_hub/index)库登录您的机器，并在终端中运行：

```py
huggingface-cli login
```

或者，您可以在笔记本或脚本中使用`login()`进行程序化登录：

```py
>>> from huggingface_hub import login
>>> login()
```

您还可以在库中的大多数加载方法（`from_pretrained`、`hf_hub_download`、`load_dataset`等）中直接从您的脚本提供`token`参数。

有关如何登录的更多详细信息，请查看[登录指南](https://huggingface.co/docs/huggingface_hub/quick-start#login)。
