---
title: SignalR HubContext
author: tdykstra
description: ハブの外部からのクライアントに通知を送信するための ASP.NET Core SignalR HubContext サービスを使用する方法について説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/13/2018
uid: signalr/hubcontext
ms.openlocfilehash: 8be888e1f7b16d65ebbaa24b618e84fca029d80b
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207954"
---
# <a name="send-messages-from-outside-a-hub"></a>ハブの外部からのメッセージを送信します。

によって[Mikael Mengistu](https://twitter.com/MikaelM_12)

SignalR ハブは、SignalR のサーバーに接続しているクライアントにメッセージを送信するための中核となる抽象化です。 アプリを使用して、その他の場所からメッセージを送信することも、`IHubContext`サービス。 この記事は、SignalR にアクセスする方法を説明します`IHubContext`ハブ外からのクライアントに通知を送信します。

[サンプル コードのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubcontext/sample/) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)

## <a name="get-an-instance-of-ihubcontext"></a>IHubContext のインスタンスを取得します。

ASP.NET Core signalr でのインスタンスにアクセスすることができます`IHubContext`依存関係の挿入を使用しています。 インスタンスを挿入できる`IHubContext`コント ローラー、ミドルウェア、またはその他の DI サービスにします。 インスタンスを使用して、クライアントにメッセージを送信します。

> [!NOTE]
> 一方、ASP.NET 4.x GlobalHost にアクセスできるようにするために使用する SignalR、`IHubContext`します。 ASP.NET Core は、このグローバル シングルトンの必要性を削除する依存関係挿入フレームワークです。

### <a name="inject-an-instance-of-ihubcontext-in-a-controller"></a>IHubContext のコント ローラーのインスタンスを挿入します。

インスタンスを挿入できる`IHubContext`コンス トラクターに追加して、コント ローラーにします。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=12-19,57)]

現在のインスタンスへのアクセスで`IHubContext`、ハブ自体で必要がある場合は、ハブ メソッドを呼び出すことができます。

[!code-csharp[IHubContext](hubcontext/sample/Controllers/HomeController.cs?range=21-25)]

### <a name="get-an-instance-of-ihubcontext-in-middleware"></a>IHubContext のミドルウェア内でインスタンスを取得します。

アクセス、`IHubContext`ミドルウェア パイプライン内で次のようにします。

```csharp
app.Use(next => async (context) =>
{
    var hubContext = context.RequestServices
                            .GetRequiredService<IHubContext<MyHub>>();
    //...
});
```

> [!NOTE]
> ハブ メソッドの外部から呼び出されるときに、`Hub`クラスは、呼び出しに関連付けられている呼び出し元はありません。 そのためへのアクセスはありません、 `ConnectionId`、 `Caller`、および`Others`プロパティ。

## <a name="related-resources"></a>関連資料

* [開始するには](xref:tutorials/signalr)
* [ハブ](xref:signalr/hubs)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
