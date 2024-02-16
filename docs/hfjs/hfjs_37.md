# @huggingface/hub

> 原文链接: [`huggingface.co/docs/huggingface.js/hub/modules`](https://huggingface.co/docs/huggingface.js/hub/modules)

## 类

+   HubApiError

+   InvalidApiResponseFormatError

## 接口

+   AuthInfo

+   CommitDeletedEntry

+   CommitFile

+   CommitOutput

+   CommitParams

+   Credentials

+   DatasetEntry

+   FileDownloadInfoOutput

+   ListFileEntry

+   ModelEntry

+   OAuthResult

+   RepoId

+   SafetensorsIndexJson

+   SpaceResourceConfig

+   SpaceResourceRequirement

+   SpaceRuntime

+   TensorInfo

+   WhoAmIApp

+   WhoAmIOrg

+   WhoAmIUser

## 类型别名

### AccessToken

Ƭ `AccessToken`: `string`

实际上是 `hf_${string}`, 但为了方便起见，使用字符串类型

#### 定义在

[hub/src/types/public.d.ts:15](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L15)

* * *

### AccessTokenRole

Ƭ `AccessTokenRole`: `"admin"` | `"write"` | `"contributor"` | `"read"`

#### 定义在

[hub/src/types/public.d.ts:44](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L44)

* * *

### AuthType

Ƭ `AuthType`: `"access_token"` | `"app_token"` | `"app_token_as_user"`

#### 定义在

[hub/src/types/public.d.ts:46](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L46)

* * *

### CommitOperation

Ƭ `CommitOperation`: `CommitDeletedEntry` | `CommitFile`

#### 定义在

[hub/src/lib/commit.ts:53](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L53)

* * *

### CommitProgressEvent

Ƭ `CommitProgressEvent`: { `event`: `"phase"` ; `phase`: `"preuploading"` | `"uploadingLargeFiles"` | `"committing"` } | { `event`: `"fileProgress"` ; `path`: `string` ; `progress`: `number` ; `state`: `"hashing"` | `"uploading"` }

#### 定义在

[hub/src/lib/commit.ts:105](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L105)

* * *

### ContentSource

Ƭ `ContentSource`: `Blob` | `URL`

#### 定义在

[hub/src/lib/commit.ts:34](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L34)

* * *

### Dtype

Ƭ `Dtype`: `"F64"` | `"F32"` | `"F16"` | `"BF16"` | `"I64"` | `"I32"` | `"I16"` | `"I8"` | `"U8"` | `"BOOL"`

#### 定义在

[hub/src/lib/parse-safetensors-metadata.ts:21](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L21)

* * *

### PipelineType

Ƭ `PipelineType`: keyof typeof `PIPELINE_DATA`

#### 定义在

tasks/dist/index.d.ts:444

* * *

### RepoDesignation

Ƭ `RepoDesignation`: `RepoId` | `RepoFullName`

#### 定义在

[hub/src/types/public.d.ts:12](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L12)

* * *

### RepoFullName

Ƭ `RepoFullName`: `string` | `spaces/${string}` | `datasets/${string}`

#### 定义在

[hub/src/types/public.d.ts:10](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L10)

* * *

### RepoType

Ƭ `RepoType`: `"space"` | `"dataset"` | `"model"`

#### 定义在

[hub/src/types/public.d.ts:3](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L3)

* * *

### 安全张量文件头

Ƭ `SafetensorsFileHeader`: `Record`<`TensorName`, `TensorInfo`> & { `__metadata__`: `Record`<`string`, `string`> }

#### 定义在

[hub/src/lib/parse-safetensors-metadata.ts:29](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L29)

* * *

### SafetensorsParseFromRepo

Ƭ `SafetensorsParseFromRepo`: { `header`: `SafetensorsFileHeader` ; `parameterCount?`: `Partial`<`Record`<`Dtype`, `number`>> ; `sharded`: `false` } | { `headers`: `SafetensorsShardedHeaders` ; `index`: `SafetensorsIndexJson` ; `parameterCount?`: `Partial`<`Record`<`Dtype`, `number`>> ; `sharded`: `true` }

#### 定义在

[hub/src/lib/parse-safetensors-metadata.ts:43](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L43)

* * *

### SafetensorsShardedHeaders

Ƭ `SafetensorsShardedHeaders`: `Record`<`FileName`, `SafetensorsFileHeader`>

#### 定义在

[hub/src/lib/parse-safetensors-metadata.ts:41](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L41)

* * *

### SpaceEntry

Ƭ `SpaceEntry`: { `id`: `string` ; `likes`: `number` ; `name`: `string` ; `private`: `boolean` ; `sdk?`: `SpaceSdk` ; `updatedAt`: `Date` } & `Partial`<`Omit`<`ApiSpaceInfo`, `"updatedAt"`>>

#### 定义在

[hub/src/lib/list-spaces.ts:11](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-spaces.ts#L11)

* * *

### SpaceHardwareFlavor

Ƭ `SpaceHardwareFlavor`: `"cpu-basic"` | `"cpu-upgrade"` | `"t4-small"` | `"t4-medium"` | `"a10g-small"` | `"a10g-large"` | `"a100-large"`

#### 定义在

[hub/src/types/public.d.ts:21](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L21)

* * *

### SpaceSdk

Ƭ `SpaceSdk`: `"streamlit"` | `"gradio"` | `"docker"` | `"static"`

#### 定义在

[hub/src/types/public.d.ts:30](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L30)

* * *

### SpaceStage

Ƭ `SpaceStage`: `"NO_APP_FILE"` | `"CONFIG_ERROR"` | `"BUILDING"` | `"BUILD_ERROR"` | `"RUNNING"` | `"RUNNING_BUILDING"` | `"RUNTIME_ERROR"` | `"DELETING"` | `"PAUSED"` | `"SLEEPING"`

#### 定义在

[hub/src/types/public.d.ts:32](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/types/public.d.ts#L32)

* * *

### TensorName

Ƭ `TensorName`: `string`

#### 定义在

[hub/src/lib/parse-safetensors-metadata.ts:20](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L20)

* * *

### WhoAmI

Ƭ `WhoAmI`: `WhoAmIApp` | `WhoAmIOrg` | `WhoAmIUser`

#### 定义在

[hub/src/lib/who-am-i.ts:50](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/who-am-i.ts#L50)

## 函数

### 提交

▸ `commit`(`params`): `Promise`<`CommitOutput`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `CommitParams` |

#### 返回

`Promise`<`CommitOutput`>

#### 定义在

[hub/src/lib/commit.ts:552](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L552)

* * *

### commitIter

▸ `commitIter`(`params`): `AsyncGenerator`<`CommitProgressEvent`, `CommitOutput`>

目前是内部函数，由提交使用。

可以稍后暴露以提供精细调整的进度信息

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `CommitParams` |

#### 返回

`AsyncGenerator`<`CommitProgressEvent`, `CommitOutput`>

#### 定义在

[hub/src/lib/commit.ts:122](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/commit.ts#L122)

* * *

### createRepo

▸ `createRepo`(`params`): `Promise`<{ `repoUrl`: `string` }>

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `Object` | - |
| `params.credentials` | `凭证` | - |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义的获取函数，用于替代默认的函数，例如使用代理或编辑标头。 |
| `params.files?` | { `content`: `ArrayBuffer` &#124; `Blob` ; `path`: `字符串` }[] | 仅支持一些轻量级文件的存储 |
| `params.hubUrl?` | `字符串` | - |
| `params.license?` | `字符串` | - |
| `params.private?` | `布尔值` | - |
| `params.repo` | `RepoDesignation` | - |
| `params.sdk?` | `SpaceSdk` | **`必需`** 当 repo.type === “space” 时 |

#### 返回

`Promise`<{ `repoUrl`: `字符串` }>

#### 定义于

[hub/src/lib/create-repo.ts:9](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/create-repo.ts#L9)

* * *

### deleteFile

▸ `deleteFile`(`params`): `Promise`<`CommitOutput`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `对象` |
| `params.branch?` | `字符串` |
| `params.commitDescription?` | `字符串` |
| `params.commitTitle?` | `字符串` |
| `params.credentials` | `凭证` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `字符串` |
| `params.isPullRequest?` | `布尔值` |
| `params.parentCommit?` | `字符串` |
| `params.path` | `字符串` |
| `params.repo` | `RepoDesignation` |

#### 返回

`Promise`<`CommitOutput`>

#### 定义于

[hub/src/lib/delete-file.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/delete-file.ts#L5)

* * *

### deleteFiles

▸ `deleteFiles`(`params`): `Promise`<`CommitOutput`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `对象` |
| `params.branch?` | `字符串` |
| `params.commitDescription?` | `字符串` |
| `params.commitTitle?` | `字符串` |
| `params.credentials` | `凭证` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `字符串` |
| `params.isPullRequest?` | `布尔值` |
| `params.parentCommit?` | `字符串` |
| `params.paths` | `字符串`[] |
| `params.repo` | `RepoDesignation` |

#### 返回

`Promise`<`CommitOutput`>

#### 定义于

[hub/src/lib/delete-files.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/delete-files.ts#L5)

* * *

### deleteRepo

▸ `deleteRepo`(`params`): `Promise`<`空`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `对象` |
| `params.credentials` | `凭证` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `字符串` |
| `params.repo` | `RepoDesignation` |

#### 返回

`Promise`<`空`>

#### 定义于

[hub/src/lib/delete-repo.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/delete-repo.ts#L7)

* * *

### downloadFile

▸ `downloadFile`(`params`): `Promise`<`Response` | `null`>

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `对象` | - |
| `params.credentials?` | `凭证` | - |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义的获取函数，用于替代默认的函数，例如使用代理或编辑标头。 |
| `params.hubUrl?` | `字符串` | - |
| `params.path` | `字符串` | - |
| `params.range?` | [`数字`, `数字`] | 仅获取文件的特定部分 |
| `params.raw?` | `布尔值` | 如果为真，将下载原始的 git 文件。例如，当调用存储在 Git LFS 中的文件时，将下载指针文件。 |
| `params.repo` | `RepoDesignation` | - |
| `params.revision?` | `string` | - |

#### 返回

`Promise`<`Response` | `null`>

当文件不存在时为 null

#### 定义在

[hub/src/lib/download-file.ts:10](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/download-file.ts#L10)

* * *

### fileDownloadInfo

▸ `fileDownloadInfo`(`params`): `Promise`<`FileDownloadInfoOutput` | `null`>

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `Object` | - |
| `params.credentials?` | `Credentials` | - |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义 fetch 函数，用于替代默认的 fetch 函数，例如使用代理或编辑标头。 |
| `params.hubUrl?` | `string` | - |
| `params.noContentDisposition?` | `boolean` | 避免在 LFS 文件的`downloadLink`中使用 content-disposition 标头，以便在浏览器中可以将 URL 用作 iframe 的例子 |
| `params.path` | `string` | - |
| `params.raw?` | `boolean` | 获取 LFS 文件背后的原始指针文件 |
| `params.repo` | `RepoDesignation` | - |
| `params.revision?` | `string` | - |

#### 返回

`Promise`<`FileDownloadInfoOutput` | `null`>

当文件不存在时为 null

#### 定义在

[hub/src/lib/file-download-info.ts:18](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/file-download-info.ts#L18)

* * *

### fileExists

▸ `fileExists`(`params`): `Promise`<`boolean`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `Object` |
| `params.credentials?` | `Credentials` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `string` |
| `params.path` | `string` |
| `params.repo` | `RepoDesignation` |
| `params.revision?` | `string` |

#### 返回

`Promise`<`boolean`>

#### 定义在

[hub/src/lib/file-exists.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/file-exists.ts#L7)

* * *

### listDatasets

▸ `listDatasets`(`params?`): `AsyncGenerator`<`DatasetEntry`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params?` | `Object` |
| `params.credentials?` | `Credentials` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `string` |
| `params.search?` | `Object` |
| `params.search.owner?` | `string` |

#### 返回

`AsyncGenerator`<`DatasetEntry`>

#### 定义在

[hub/src/lib/list-datasets.ts:20](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-datasets.ts#L20)

* * *

### listFiles

▸ `listFiles`(`params`): `AsyncGenerator`<`ListFileEntry`>

列出文件夹中的文件。要列出目录中的所有文件，请将 params.recursive 设置为`true`。

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `Object` | - |
| `params.credentials?` | `Credentials` | - |
| `params.expand?` | `boolean` | 获取每个文件的`lastCommit`和`securityStatus`。 |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义 fetch 函数，用于替代默认的 fetch 函数，例如使用代理或编辑标头。 |
| `params.hubUrl?` | `string` | - |
| `params.path?` | `string` | 例如‘data’表示列出‘data’文件夹中的所有文件。将其留空以列出存储库中的所有文件。 |
| `params.recursive?` | `boolean` | 我们是否要列出子目录中的文件？ |
| `params.repo` | `RepoDesignation` | - |
| `params.revision?` | `string` | - |

#### 返回

`AsyncGenerator`<`ListFileEntry`>

#### 定义在

[hub/src/lib/list-files.ts:38](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-files.ts#L38)

* * *

### listModels

▸ `listModels`(`params?`): `AsyncGenerator`<`ModelEntry`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params?` | `Object` |
| `params.credentials?` | `Credentials` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `string` |
| `params.search?` | `Object` |
| `params.search.owner?` | `string` |
| `params.search.task?` | `"text-classification"` &#124; `"token-classification"` &#124; `"table-question-answering"` &#124; `"question-answering"` &#124; `"zero-shot-classification"` &#124; `"translation"` &#124; `"summarization"` &#124; `"conversational"` &#124; `"feature-extraction"` &#124; `"text-generation"` &#124; `"text2text-generation"` &#124; `"fill-mask"` &#124; `"sentence-similarity"` &#124; `"text-to-speech"` &#124; `"text-to-audio"` &#124; `"automatic-speech-recognition"` &#124; `"audio-to-audio"` &#124; `"audio-classification"` &#124; `"voice-activity-detection"` &#124; `"depth-estimation"` &#124; `"image-classification"` &#124; `"object-detection"` &#124; `"image-segmentation"` &#124; `"text-to-image"` &#124; `"image-to-text"` &#124; `"image-to-image"` &#124; `"image-to-video"` &#124; `"unconditional-image-generation"` &#124; `"video-classification"` &#124; `"reinforcement-learning"` &#124; `"robotics"` &#124; `"tabular-classification"` &#124; `"tabular-regression"` &#124; `"tabular-to-text"` &#124; `"table-to-text"` &#124; `"multiple-choice"` &#124; `"text-retrieval"` &#124; `"time-series-forecasting"` &#124; `"text-to-video"` &#124; `"visual-question-answering"` &#124; `"document-question-answering"` &#124; `"zero-shot-image-classification"` &#124; `"graph-ml"` &#124; `"mask-generation"` &#124; `"zero-shot-object-detection"` &#124; `"text-to-3d"` &#124; `"image-to-3d"` &#124; `"other"` |

#### 返回

`AsyncGenerator`<`ModelEntry`>

#### 定义于

[hub/src/lib/list-models.ts:28](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-models.ts#L28)

* * *

### listSpaces

▸ `listSpaces`(`params?`): `AsyncGenerator`<`SpaceEntry`>

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params?` | `Object` | - |
| `params.additionalFields?` | keyof `ApiSpaceInfo`[] | 从 huggingface.co 获取的额外字段。 |
| `params.credentials?` | `Credentials` | - |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义 fetch 函数，用于替代默认的 fetch 函数，例如使用代理或编辑标头。 |
| `params.hubUrl?` | `string` | - |
| `params.search?` | `Object` | - |
| `params.search.owner?` | `string` | - |

#### 返回

`AsyncGenerator`<`SpaceEntry`>

#### 定义于

[hub/src/lib/list-spaces.ts:21](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/list-spaces.ts#L21)

* * *

### oauthHandleRedirect

▸ `oauthHandleRedirect`(`opts?`): `Promise`<`OAuthResult`>

在 OAuth 提供程序重定向回应用程序后调用。

还有一个辅助函数 oauthHandleRedirectIfPresent，如果 URL 中包含查询参数中的 oauth 代码，则调用`oauthHandleRedirect`，否则返回`false`。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `opts?` | `Object` |
| `opts.hubUrl?` | `string` |

#### 返回

`Promise`<`OAuthResult`>

#### 定义于

[hub/src/lib/oauth-handle-redirect.ts:37](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L37)

* * *

### oauthHandleRedirectIfPresent

▸ `oauthHandleRedirectIfPresent`(`opts?`): `Promise`<`OAuthResult` | `false`>

在 OAuth 提供程序重定向回应用程序后调用。

如果 URL 不包含查询参数中的 oauth 代码，则返回 false，否则调用 oauthHandleRedirect。 

根据您的应用程序，您可能希望直接调用 oauthHandleRedirect。

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `opts?` | `Object` |
| `opts.hubUrl?` | `string` |

#### 返回

`Promise`<`OAuthResult` | `false`>

#### 定义于

[hub/src/lib/oauth-handle-redirect.ts:191](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L191)

* * *

### oauthLoginUrl

▸ `oauthLoginUrl`(`opts?`): `Promise`<`string`>

使用“使用 Hub 登录”来认证用户，并获取 oauth 用户信息/访问令牌。

返回一个重定向的 URL。用户重定向回您的应用程序后，调用`oauthHandleRedirect`来获取 oauth 用户信息/访问令牌。

当从启用 OAuth 的静态空间内调用时，将从空间加载配置，否则您至少需要指定 OAuth 应用程序的客户端 ID。

**`示例`**

```py
import { oauthLoginUrl, oauthHandleRedirectIfPresent } from "@huggingface/hub";

const oauthResult = await oauthHandleRedirectIfPresent();

if (!oauthResult) {
  // If the user is not logged in, redirect to the login page
  window.location.href = await oauthLoginUrl();
}

// You can use oauthResult.accessToken, oauthResult.accessTokenExpiresAt and oauthResult.userInfo
console.log(oauthResult);
```

（理论上，此函数可以用于通过更改`hubUrl`来认证支持 PKCE 和 OpenID Connect 的任何 OAuth 提供程序的用户，但目前仅与 Hugging Face Hub 进行了测试。）

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `opts?` | `Object` | - |
| `opts.clientId?` | `string` | OAuth 客户端 ID。对于静态空间，您可以省略这一点，只要 README.md 的元数据中存在`hf_oauth: true`，它将从空间配置中加载。对于其他空间，只要 README.md 的元数据中存在`hf_oauth: true`，它就可以在后端的 OAUTH_CLIENT_ID 环境变量中使用。您还可以在[`huggingface.co/settings/connected-applications`](https://huggingface.co/settings/connected-applications)创建开发者应用程序并使用其客户端 ID。 |
| `opts.hubUrl?` | `string` | - |
| `opts.redirectUrl?` | `string` | 重定向 URI，默认为当前 URL。对于空间，允许在空间内的任何 URL。对于开发者应用程序，您可以在[`huggingface.co/settings/connected-applications`](https://huggingface.co/settings/connected-applications)中将任何 URL 添加到允许的重定向 URI 列表中。 |
| `opts.scopes?` | `string` | OAuth 范围，一个以空格分隔的范围列表。对于静态空间，您可以省略这一点，它将从空间配置中加载，只要 README.md 的元数据中存在`hf_oauth: true`。对于其他空间，只要 README.md 的元数据中存在`hf_oauth: true`，它就可以在后端的 OAUTH_SCOPES 环境变量中使用。默认为“openid profile”。您还可以在[`huggingface.co/settings/connected-applications`](https://huggingface.co/settings/connected-applications)创建开发者应用程序并使用其范围。请参阅[`huggingface.co/docs/hub/oauth`](https://huggingface.co/docs/hub/oauth)获取可用范围的列表。 |
| `opts.state?` | `string` | 传递给 OAuth 提供程序的状态，在重定向后将在调用`oauthLogin`时返回。 |

#### 返回

`Promise`<`string`>

#### 定义于

[hub/src/lib/oauth-login-url.ts:31](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-login-url.ts#L31)

* * *

### parseSafetensorsMetadata

▸ `parseSafetensorsMetadata`(`params`): `Promise`<`SetRequired`<`SafetensorsParseFromRepo`, `"parameterCount"`>>

使用智能范围请求从 Hugging Face 托管的模型的 model.safetensors.index.json 或 model.safetensors 中提取其元数据。

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `Object` | - |
| `params.computeParametersCount` | `true` | 将包括 SafetensorsParseFromRepo["parameterCount"]，一个包含每种 DType 参数数量的对象 **`默认`** 为 false |
| `params.credentials?` | `Credentials` | - |
| `params.fetch?` | (`input`: `URL` 或 `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义 fetch 函数，用于替代默认函数，例如使用代理或编辑标头。 |
| `params.hubUrl?` | `string` | - |
| `params.repo` | `RepoDesignation` | 仅支持模型 |
| `params.revision?` | `string` | - |

#### 返回

`Promise`<`SetRequired`<`SafetensorsParseFromRepo`, `"parameterCount"`>>

#### 定义于

[hub/src/lib/parse-safetensors-metadata.ts:148](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L148)

▸ `parseSafetensorsMetadata`(`params`): `Promise`<`SafetensorsParseFromRepo`>

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `Object` | - |
| `params.computeParametersCount?` | `boolean` | 将包括 SafetensorsParseFromRepo[“parameterCount”]，一个包含每个 DType 参数数量的对象 **`默认`** 为 false |
| `params.credentials?` | `Credentials` | - |
| `params.fetch?` | (`input`: `URL` 或 `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> | 自定义 fetch 函数，用于替代默认函数，例如使用代理或编辑标头。 |
| `params.hubUrl?` | `string` | - |
| `params.repo` | `RepoDesignation` | 仅支持模型 |
| `params.revision?` | `string` | - |

#### 返回

`Promise`<`SafetensorsParseFromRepo`>

#### 定义于

[hub/src/lib/parse-safetensors-metadata.ts:165](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/parse-safetensors-metadata.ts#L165)

* * *

### uploadFile

▸ `uploadFile`(`params`): `Promise`<`CommitOutput`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `Object` |
| `params.abortSignal?` | `AbortSignal` |
| `params.branch?` | `string` |
| `params.commitDescription?` | `string` |
| `params.commitTitle?` | `string` |
| `params.credentials?` | `Credentials` |
| `params.fetch?` | (`input`: `URL` 或 `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.file` | `URL` 或 `File` 或 { `content`: `ContentSource` ; `path`: `string` } |
| `params.hubUrl?` | `string` |
| `params.isPullRequest?` | `boolean` |
| `params.parentCommit?` | `string` |
| `params.repo` | `RepoDesignation` |
| `params.useWebWorkers?` | `boolean` 或 { `minSize?`: `number` ; `poolSize?`: `number` } |

#### 返回

`Promise`<`CommitOutput`>

#### 定义于

[hub/src/lib/upload-file.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/upload-file.ts#L5)

* * *

### uploadFiles

▸ `uploadFiles`(`params`): `Promise`<`CommitOutput`>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `Object` |
| `params.abortSignal?` | `AbortSignal` |
| `params.branch?` | `string` |
| `params.commitDescription?` | `string` |
| `params.commitTitle?` | `string` |
| `params.credentials?` | `Credentials` |
| `params.fetch?` | (`input`: `URL` 或 `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.files` | (`URL` 或 `File` 或 { `content`: `ContentSource` ; `path`: `string` })[] |
| `params.hubUrl?` | `string` |
| `params.isPullRequest?` | `boolean` |
| `params.parentCommit?` | `string` |
| `params.repo` | `RepoDesignation` |
| `params.useWebWorkers?` | `boolean` 或 { `minSize?`: `number` ; `poolSize?`: `number` } |

#### 返回

`Promise`<`CommitOutput`>

#### 定义于

[hub/src/lib/upload-files.ts:4](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/upload-files.ts#L4)

* * *

### uploadFilesWithProgress

▸ `uploadFilesWithProgress`(`params`): `AsyncGenerator`<`CommitProgressEvent`, `CommitOutput`>

上传带有进度

需要 XMLHttpRequest 可用于上传的进度事件 设置 useWebWorkers 为 true 以获得哈希的进度事件

#### 参数

| 名称 | 类型 | 描述 |
| :-- | :-- | :-- |
| `params` | `Object` | - |
| `params.abortSignal?` | `AbortSignal` | - |
| `params.branch?` | `string` | - |
| `params.commitDescription?` | `string` | - |
| `params.commitTitle?` | `string` | - |
| `params.credentials?` | `Credentials` | - |
| `params.files` | (`URL` &#124; `File` &#124; { `content`: `ContentSource` ; `path`: `string` })[] | - |
| `params.hubUrl?` | `string` | - |
| `params.isPullRequest?` | `boolean` | - |
| `params.parentCommit?` | `string` | - |
| `params.repo` | `RepoDesignation` | - |
| `params.useWebWorkers?` | `boolean` &#124; { `minSize?`: `number` ; `poolSize?`: `number` } | 将其设置为 true 以获得哈希进度事件 |

#### 返回

`AsyncGenerator`<`CommitProgressEvent`, `CommitOutput`>

#### 定义在

[hub/src/lib/upload-files-with-progress.ts:19](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/upload-files-with-progress.ts#L19)

* * *

### whoAmI

▸ `whoAmI`(`params`): `Promise`<`WhoAmI` & { `auth`: `AuthInfo` }>

#### 参数

| 名称 | 类型 |
| :-- | :-- |
| `params` | `Object` |
| `params.credentials` | `Credentials` |
| `params.fetch?` | (`input`: `URL` &#124; `RequestInfo`, `init?`: `RequestInit`) => `Promise`<`Response`> |
| `params.hubUrl?` | `string` |

#### 返回

`Promise`<`WhoAmI` & { `auth`: `AuthInfo` }>

#### 定义在

[hub/src/lib/who-am-i.ts:61](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/who-am-i.ts#L61)
