# 🤗 Hugging Face Hub API

> 原始文本：[https://huggingface.co/docs/huggingface.js/hub/README](https://huggingface.co/docs/huggingface.js/hub/README)

官方实用程序用于使用 Hugging Face hub API，仍然非常实验性。

## 安装

```py
pnpm add @huggingface/hub

npm add @huggingface/hub

yarn add @huggingface/hub
```

### Deno

```py
// esm.sh
import { uploadFiles, listModels } from "https://esm.sh/@huggingface/hub"
// or npm:
import { uploadFiles, listModels } from "npm:@huggingface/hub"
```

## 用法

对于某些调用，您需要创建帐户并生成一个[访问令牌](https://huggingface.co/settings/tokens)。

学习如何使用 hub 包在此[交互式教程](https://scrimba.com/scrim/c7BbVPcd?pl=pkVnrP7uP)中查找免费模型。

```py
import { createRepo, uploadFiles, uploadFilesWithProgress, deleteFile, deleteRepo, listFiles, whoAmI } from "@huggingface/hub";
import type { RepoDesignation, Credentials } from "@huggingface/hub";

const repo: RepoDesignation = { type: "model", name: "myname/some-model" };
const credentials: Credentials = { accessToken: "hf_..." };

const {name: username} = await whoAmI({credentials});

for await (const model of listModels({search: {owner: username}, credentials})) {
  console.log("My model:", model);
}

await createRepo({ repo, credentials, license: "mit" });

await uploadFiles({
  repo,
  credentials,
  files: [
    // path + blob content
    {
      path: "file.txt",
      content: new Blob(["Hello World"]),
    },
    // Local file URL
    pathToFileURL("./pytorch-model.bin"),
    // Web URL
    new URL("https://huggingface.co/xlm-roberta-base/resolve/main/tokenizer.json"),
    // Path + Web URL
    {
      path: "myfile.bin",
      content: new URL("https://huggingface.co/bert-base-uncased/resolve/main/pytorch_model.bin")
    }
    // Can also work with native File in browsers
  ],
});

// or

for await (const progressEvent of await uploadFilesWithProgress({
  repo,
  credentials,
  files: [
    ...
  ],
})) {
  console.log(progressEvent);
}

await deleteFile({repo, credentials, path: "myfile.bin"});

await (await downloadFile({ repo, path: "README.md" })).text();

for await (const fileInfo of listFiles({repo})) {
  console.log(fileInfo);
}

await deleteRepo({ repo, credentials });
```

## OAuth 登录

可以使用 OAuth 进行登录（[“使用 HF 登录”](https://huggingface.co/docs/hub/oauth)）。

这将允许您获取访问令牌，以便根据 Space 或 OAuth 应用程序中设置的范围使用 API 的某些功能。

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

查看演示：[https://huggingface.co/spaces/huggingfacejs/client-side-oauth](https://huggingface.co/spaces/huggingfacejs/client-side-oauth)

## 性能考虑

在上传大文件时，您可能希望在工作线程中运行 `commit` 调用，以卸载 sha256 计算。

远程资源和本地文件应尽可能作为 `URL` 传递，以便它们可以按块懒加载以减少 RAM 使用量。在浏览器上下文中传递 `File` 是可以的，因为它在本质上表现为 `Blob`。

在底层，`@huggingface/hub` 使用了一种懒加载的 blob 实现来加载文件。

## 依赖项

+   `hash-wasm`：仅在浏览器中使用，用于提交超过 10 MB 的文件。浏览器不原生支持流式 sha256 计算。

+   `type-fest`：仅类型
