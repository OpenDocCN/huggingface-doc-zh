# 接口：OAuthResult

> 原始文本：[`huggingface.co/docs/huggingface.js/hub/interfaces/OAuthResult`](https://huggingface.co/docs/huggingface.js/hub/interfaces/OAuthResult)

## 属性

### accessToken

• `accessToken`: `string`

#### 定义在

[hub/src/lib/oauth-handle-redirect.ts:5](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L5)

* * *

### accessTokenExpiresAt

• `accessTokenExpiresAt`: `Date`

#### 定义在

[hub/src/lib/oauth-handle-redirect.ts:6](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L6)

* * *

### 范围

• `scope`: `string`

授予的范围

#### 定义在

[hub/src/lib/oauth-handle-redirect.ts:28](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L28)

* * *

### 状态

• `可选` `state`: `string`

在原始请求中传递给 OAuth 提供程序的状态。

#### 定义在

[hub/src/lib/oauth-handle-redirect.ts:24](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L24)

* * *

### userInfo

• `userInfo`: `Object`

#### 类型声明

| 名称 | 类型 |
| :-- | :-- |
| `avatarUrl` | `string` |
| `email?` | `string` |
| `emailVerified?` | `boolean` |
| `fullname` | `string` |
| `id` | `string` |
| `isPro` | `boolean` |
| `name` | `string` |
| `orgs` | { `isEnterprise`: `boolean` ; `name`: `string` }[] |
| `websiteUrl?` | `string` |

#### 定义在

[hub/src/lib/oauth-handle-redirect.ts:7](https://github.com/huggingface/huggingface.js/blob/main/packages/hub/src/lib/oauth-handle-redirect.ts#L7)
