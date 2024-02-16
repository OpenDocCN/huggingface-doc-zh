# 在数据集中搜索文本

> 原始文本：[https://huggingface.co/docs/datasets-server/search](https://huggingface.co/docs/datasets-server/search)

数据集服务器提供了用于在数据集中搜索单词的`/search`端点。

目前，仅支持具有Parquet导出的数据集，因此数据集服务器可以索引内容并在不下载整个数据集的情况下运行搜索。

本指南向您展示如何使用数据集服务器的`/search`端点来搜索查询字符串。也可以尝试使用[ReDoc](https://redocly.github.io/redoc/?url=https://datasets-server.huggingface.co/openapi.json#operation/searchRows)。

在`string`类型的列中搜索文本，即使值嵌套在字典中。

`/search`端点接受五个查询参数：

+   `dataset`：数据集名称，例如`glue`或`mozilla-foundation/common_voice_10_0`

+   `config`：配置名称，例如`cola`

+   `split`：拆分名称，例如`train`

+   `query`：要搜索的文本

+   `offset`：切片的偏移量，例如`150`

+   `length`：切片的长度，例如`10`（最大值：`100`）

例如，让我们在`ibm/duorc`数据集的`SelfRC`配置的`train`拆分中搜索文本`"dog"`，将结果限制为切片150-151：

PythonJavaScriptcURL

```py
import requests
headers = {"Authorization": f"Bearer {API_TOKEN}"}
API_URL = "https://datasets-server.huggingface.co/search?dataset=ibm/duorc&config=SelfRC&split=train&query=dog&offset=150&length=2"
def query():
    response = requests.get(API_URL, headers=headers)
    return response.json()
data = query()
```

端点响应是一个包含两个键的JSON（与[`/rows`](./rows)的格式相同）：

+   数据集的[`features`](https://huggingface.co/docs/datasets/about_dataset_features)，包括列名和数据类型。

+   数据集的`rows`切片和特定行每列中包含的内容。

行按行索引排序，与查询匹配的文本字符串未突出显示。

例如，这里是匹配查询`dog`的`duorc`/`SelfRC`训练拆分的`features`和切片150-151的`rows`：

```py
{
  "features": [
    {
      "feature_idx": 0,
      "name": "plot_id",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 1,
      "name": "plot",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 2,
      "name": "title",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 3,
      "name": "question_id",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 4,
      "name": "question",
      "type": { "dtype": "string", "_type": "Value" }
    },
    {
      "feature_idx": 5,
      "name": "answers",
      "type": {
        "feature": { "dtype": "string", "_type": "Value" },
        "_type": "Sequence"
      }
    },
    {
      "feature_idx": 6,
      "name": "no_answer",
      "type": { "dtype": "bool", "_type": "Value" }
    }
  ],
  "rows": [
    {
      "row_idx": 1561,
      "row": {
        "plot_id": "/m/014bjk",
        "plot": "The film begins with clips that track a telephone call between London and Geneva, where a university student and part-time model, Valentine Dussault (IrÃ¨ne Jacob), is talking to her emotionally infantile and possessive boyfriend. During her work as a model she poses for a chewing-gum campaign and during the photo shoot the photographer asks her to look very sad. While walking back home, Auguste, a neighbour of Valentine's, drops a set of books, notices that a particular chapter of the Criminal Code opened at random, and concentrates on that passage. As she drives back to her apartment, Valentine is distracted while adjusting the radio and accidentally hits a dog. She tracks down the owner, a reclusive retired judge, Joseph Kern (Jean-Louis Trintignant). He seems unconcerned by the accident or the injuries sustained by Rita, his dog. Valentine takes Rita to a veterinarian, where she learns that Rita is pregnant. Valentine takes the dog home. Later, money is delivered to her apartment from an unnamed sender.\nWhilst Valentine is walking Rita the next day the dog runs away and Valentine eventually finds her back at Kern's house. She asks and he confirms that the money sent to her came from him, for the vet bill. He then tells Valentine she can have the dog. A short time later Valentine finds Kern eavesdropping on his neighbours' private telephone conversations. The judge challenges Valentine to go tell the neighbours and initially she goes to do so. She visits the neighbours' house, which appears, on the surface, to contain a contented nuclear family, causing her to change her mind about exposing their secrets. She returns to Kern's house and Kern tells her that it would make no difference if she denounced him for his spying because the people's lives he listens to would eventually turn into hell anyway. She leaves saying that she feels nothing but pity for him.\nWhilst visiting Kern, Valentine hears a phone conversation between her (unbeknownst to her) neighbour, Auguste, and his girlfriend, Karin (Frederique Feder). They discuss if they should go bowling. Valentine covers her ears but from the very little she hears she concludes that they love each other. Kern disagrees. That evening Valentine is alone at home and hopes that her boyfriend will call, but it is the photographer who calls, saying that her billboard was set up that evening and asks her to join them bowling to celebrate. Later, Auguste takes his exam and passes it and becomes a judge. Karin asks if he was asked any questions regarding the article that was open when he dropped his books. Auguste says yes. Karin gives him a fancy fountain pen as a gift and he wonders what the first judgment he signs with it will be. That evening, Kern writes a series of letters to his neighbours and the court confessing his activities, and the community files a class action. Later, at the law courts, he sees Karin make the acquaintance of and begin to flirt with another man. Earlier, Auguste had missed a call from Karin and tried to call her back but got no answer.\nValentine reads the news about a retired judge who spied on his neighbours and rushes to Kern's house to tell him that she did not report on him. He confesses that he turned himself in, just to see what she would do. He asks her in and shows her that Rita has had seven puppies. He tells her that in their last conversation when she spoke about pity he later realized that she really meant disgust. He ponders about the reasons why people obey laws and concludes that often it is more on selfish grounds and from fear than about obeying the law or being decent. It is his birthday and he offers her pear brandy for a toast. During their conversation he reminisces about a sailor he acquitted a long time ago, only later realizing he had made a mistake, and that the man was guilty. However, the man later married, had children and grandchildren and lives peacefully and happy. Valentine says that he did what he had to do, but Kern wonders how many other people that he acquitted or condemned might have seen a different life had he decided otherwise. Valentine tells Kern about her intended trip to England for a modeling job and to visit her boyfriend. Kern suggests that she take the ferry.\nAuguste has been unable to reach Karin since graduation so he goes to her place and sees her having sex with another man. Distraught, he leaves. Later, Auguste sees Karin and her new boyfriend in a restaurant. He gets her attention by tapping on the restaurant window with the pen she gave him. But when she rushes outside, he hides from her. In a temper, he ties his dog by a quayside and abandons him.\nKarin runs a service providing personalised weather information to travelers by telephone. Kern calls and enquires about the weather in the English Channel for the time when Valentine will be traveling to England. Karin states that she expects the weather to be perfect and reveals that she is about to take a trip there (with her new boyfriend who owns a yacht).\nThe day before Valentine leaves, she invites Kern to a fashion show where she is modeling. After the show they speak about the dream Kern had about her, where he saw her at the age of 50 and happy with an unidentified man. The conversation then turns to Kern and the reasons why he disliked Karin. Kern reveals that before becoming a judge, he was in love with a woman very much like Karin, who betrayed him for another man. While preparing for his exam, he once went to the same theatre where the fashion show took place and he accidentally dropped one of his books. When he picked it up, Kern studied the chapter where the book accidentally opened, which turned out to be the crucial question at his examination. After his girlfriend left him, he followed her across the English Channel but never saw her again, because she died in an accident. Later, he was assigned to judge a case where the defendant was the same man who took his girlfriend from him. Despite this connection, Kern did not recuse himself from the case and found the man guilty. He tells Valentine the judgment was entirely legal but also that he subsequently requested early retirement.\nValentine boards the ferry to England. Auguste is also on the ferry, clutching the dog he had temporarily abandoned. Although living in the same neighborhood and nearly crossing paths many times, the two have still never met. Suddenly a storm rises and sinks both the ferry and the boat with Karin and her boyfriend. Only seven survivors are pulled from the ferry: the main characters from the first two films of the trilogy, Julie and Olivier from Blue, Karol and Dominique from White, and Valentine and Auguste, who meet for the first time, as well as an English bartender named Stephen Killian. As in the previous films, the film's final sequence shows a character crying - in this case, the judge - but the final image replicates the iconic chewing-gum poster of Valentine, but this time with real emotion showing on her face.",
        "title": "Three Colors: Red",
        "question_id": "7c583513-0b7f-ddb3-be43-64befc7e90cc",
        "question": "Where is Valentine going on her trip?",
        "answers": ["England."],
        "no_answer": false
      },
      "truncated_cells": []
    },
    {
      "row_idx": 1562,
      "row": {
        "plot_id": "/m/014bjk",
        "plot": "The film begins with clips that track a telephone call between London and Geneva, where a university student and part-time model, Valentine Dussault (IrÃ¨ne Jacob), is talking to her emotionally infantile and possessive boyfriend. During her work as a model she poses for a chewing-gum campaign and during the photo shoot the photographer asks her to look very sad. While walking back home, Auguste, a neighbour of Valentine's, drops a set of books, notices that a particular chapter of the Criminal Code opened at random, and concentrates on that passage. As she drives back to her apartment, Valentine is distracted while adjusting the radio and accidentally hits a dog. She tracks down the owner, a reclusive retired judge, Joseph Kern (Jean-Louis Trintignant). He seems unconcerned by the accident or the injuries sustained by Rita, his dog. Valentine takes Rita to a veterinarian, where she learns that Rita is pregnant. Valentine takes the dog home. Later, money is delivered to her apartment from an unnamed sender.\nWhilst Valentine is walking Rita the next day the dog runs away and Valentine eventually finds her back at Kern's house. She asks and he confirms that the money sent to her came from him, for the vet bill. He then tells Valentine she can have the dog. A short time later Valentine finds Kern eavesdropping on his neighbours' private telephone conversations. The judge challenges Valentine to go tell the neighbours and initially she goes to do so. She visits the neighbours' house, which appears, on the surface, to contain a contented nuclear family, causing her to change her mind about exposing their secrets. She returns to Kern's house and Kern tells her that it would make no difference if she denounced him for his spying because the people's lives he listens to would eventually turn into hell anyway. She leaves saying that she feels nothing but pity for him.\nWhilst visiting Kern, Valentine hears a phone conversation between her (unbeknownst to her) neighbour, Auguste, and his girlfriend, Karin (Frederique Feder). They discuss if they should go bowling. Valentine covers her ears but from the very little she hears she concludes that they love each other. Kern disagrees. That evening Valentine is alone at home and hopes that her boyfriend will call, but it is the photographer who calls, saying that her billboard was set up that evening and asks her to join them bowling to celebrate. Later, Auguste takes his exam and passes it and becomes a judge. Karin asks if he was asked any questions regarding the article that was open when he dropped his books. Auguste says yes. Karin gives him a fancy fountain pen as a gift and he wonders what the first judgment he signs with it will be. That evening, Kern writes a series of letters to his neighbours and the court confessing his activities, and the community files a class action. Later, at the law courts, he sees Karin make the acquaintance of and begin to flirt with another man. Earlier, Auguste had missed a call from Karin and tried to call her back but got no answer.\nValentine reads the news about a retired judge who spied on his neighbours and rushes to Kern's house to tell him that she did not report on him. He confesses that he turned himself in, just to see what she would do. He asks her in and shows her that Rita has had seven puppies. He tells her that in their last conversation when she spoke about pity he later realized that she really meant disgust. He ponders about the reasons why people obey laws and concludes that often it is more on selfish grounds and from fear than about obeying the law or being decent. It is his birthday and he offers her pear brandy for a toast. During their conversation he reminisces about a sailor he acquitted a long time ago, only later realizing he had made a mistake, and that the man was guilty. However, the man later married, had children and grandchildren and lives peacefully and happy. Valentine says that he did what he had to do, but Kern wonders how many other people that he acquitted or condemned might have seen a different life had he decided otherwise. Valentine tells Kern about her intended trip to England for a modeling job and to visit her boyfriend. Kern suggests that she take the ferry.\nAuguste has been unable to reach Karin since graduation so he goes to her place and sees her having sex with another man. Distraught, he leaves. Later, Auguste sees Karin and her new boyfriend in a restaurant. He gets her attention by tapping on the restaurant window with the pen she gave him. But when she rushes outside, he hides from her. In a temper, he ties his dog by a quayside and abandons him.\nKarin runs a service providing personalised weather information to travelers by telephone. Kern calls and enquires about the weather in the English Channel for the time when Valentine will be traveling to England. Karin states that she expects the weather to be perfect and reveals that she is about to take a trip there (with her new boyfriend who owns a yacht).\nThe day before Valentine leaves, she invites Kern to a fashion show where she is modeling. After the show they speak about the dream Kern had about her, where he saw her at the age of 50 and happy with an unidentified man. The conversation then turns to Kern and the reasons why he disliked Karin. Kern reveals that before becoming a judge, he was in love with a woman very much like Karin, who betrayed him for another man. While preparing for his exam, he once went to the same theatre where the fashion show took place and he accidentally dropped one of his books. When he picked it up, Kern studied the chapter where the book accidentally opened, which turned out to be the crucial question at his examination. After his girlfriend left him, he followed her across the English Channel but never saw her again, because she died in an accident. Later, he was assigned to judge a case where the defendant was the same man who took his girlfriend from him. Despite this connection, Kern did not recuse himself from the case and found the man guilty. He tells Valentine the judgment was entirely legal but also that he subsequently requested early retirement.\nValentine boards the ferry to England. Auguste is also on the ferry, clutching the dog he had temporarily abandoned. Although living in the same neighborhood and nearly crossing paths many times, the two have still never met. Suddenly a storm rises and sinks both the ferry and the boat with Karin and her boyfriend. Only seven survivors are pulled from the ferry: the main characters from the first two films of the trilogy, Julie and Olivier from Blue, Karol and Dominique from White, and Valentine and Auguste, who meet for the first time, as well as an English bartender named Stephen Killian. As in the previous films, the film's final sequence shows a character crying - in this case, the judge - but the final image replicates the iconic chewing-gum poster of Valentine, but this time with real emotion showing on her face.",
        "title": "Three Colors: Red",
        "question_id": "80becb22-908d-84bc-3a5f-00b620d551bc",
        "question": "What was the profession of the dog's owner?",
        "answers": ["Retired Judge"],
        "no_answer": false
      },
      "truncated_cells": []
    }
  ],
  "num_rows_total": 5247,
  "num_rows_per_page": 100,
  "partial": false
}
```

如果结果为`partial: true`，意味着搜索无法在完整数据集上运行，因为数据集太大。

实际上，如果数据集大于5GB，则`/search`的索引可以是部分的。在这种情况下，它仅使用前5GB。

## 截断响应

与`/first-rows`不同，在`/search`中目前没有截断。`truncated_cells`字段仍然存在，但始终为空。
