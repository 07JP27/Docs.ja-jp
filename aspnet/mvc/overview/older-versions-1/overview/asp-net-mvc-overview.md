---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: "ASP.NET MVC の概要 |Microsoft ドキュメント"
author: microsoft
description: "ASP.NET MVC アプリケーションと ASP.NET Web フォーム アプリケーションの違いについて説明します。 ASP.NET MVC アプリケーションを構築する時期を決定する方法を説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC の概要
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC アプリケーションと ASP.NET Web フォーム アプリケーションの違いについて説明します。 ASP.NET MVC アプリケーションを構築する時期を決定する方法を説明します。


モデル ビュー コント ローラー (MVC) アーキテクチャ パターンでは、アプリケーションは、次の 3 つの主要なコンポーネントを: モデル、ビュー、およびコント ローラー。 ASP.NET MVC フレームワークは、MVC ベースの Web アプリケーションを作成するための代わりに、ASP.NET Web フォーム パターンを提供します。 ASP.NET MVC フレームワークは、軽量で高テスト可能なプレゼンテーション フレームワークを (Web フォーム ベースのアプリケーションと同様) がマスター ページやメンバーシップ ベースの認証などの既存の ASP.NET 機能と統合されています。 MVC フレームワークがで定義されている、 **System.Web.Mvc**名前空間の基本的なサポートされている一部となって、 **System.Web**名前空間。   
  
MVC は、多くの開発者が使い慣れている標準的なデザイン パターンです。 Web アプリケーションの種類によっては、MVC フレームワークからメリットがあります。 他のユーザーは、Web フォームとポストバックに基づく従来の ASP.NET アプリケーション パターンを使用して続行されます。 その他の種類の Web アプリケーションでは、2 つのアプローチを結合します。どちらアプローチでは、もう一方は除外します。   
  
MVC フレームワークには、次のコンポーネントが含まれています。


[![パラメーターの値を期待しているコント ローラー アクションの呼び出し](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**図 01**: パラメーターの値を期待しているコント ローラー アクションの呼び出し ([フルサイズのイメージを表示するをクリックして](asp-net-mvc-overview/_static/image2.png))


- **モデル**です。 モデル オブジェクトは、アプリケーションのデータ ドメインのロジックを実装するアプリケーションの部分です。 多くの場合、モデル オブジェクトは取得し、モデルの状態をデータベースに格納します。 たとえば、Product オブジェクト可能性があります、データベースから情報を取得、操作、および SQL Server の製品テーブルにバックアップの更新情報を書き込みます。

小規模なアプリケーションで使用するモデルとは、多くの場合ではなく、物理的な概念を分離です。 たとえば場合のみ、アプリケーションは、データのセットを読み取るし、ビューに送信する、アプリケーションがありません物理モデル レイヤーと関連付けられているクラス。 その場合は、データ セットは、モデル オブジェクトの役割です。

- **ビュー**です。 ビューは、アプリケーションのユーザー インターフェイス (UI) を表示するコンポーネントです。 通常、この UI は、モデル データから作成されます。 例には、テキスト ボックス、ドロップダウン リスト、および製品オブジェクトの現在の状態に基づくのチェック ボックスを表示する、Products テーブルの編集ビューがあります。

- **コント ローラー**です。 コント ローラーは、ユーザーとの対話を処理し、モデルでは、作業、最終的に表示するために UI を表示するビューを選択するコンポーネントです。 MVC アプリケーションでは、ビューのみ情報が表示されます。コント ローラーは、処理し、ユーザー入力との対話に応答します。 たとえば、コント ローラーはクエリ文字列の値を処理し、さらに、値を使用して、データベースを照会すると、モデルにこれらの値を渡します。

MVC パターンでは、これらの要素間の疎結合を提供しつつ、アプリケーション (入力ロジック、ビジネス ロジック、および UI ロジック) のさまざまな側面を分離するアプリケーションを作成できます。 パターンは、アプリケーション内の各種類のロジックの位置を指定します。 UI ロジックはビューに属しています。 入力ロジックはコントローラーに属しています。 ビジネス ロジックはモデルに属しています。 この分離では、一度に実装の 1 つの側面に焦点を有効にするため、アプリケーションをビルドすると、複雑さを管理できます。 たとえば、ビジネス ロジックに依存せず、ビューに集中できます。   
  
複雑さを管理するだけでなく、MVC パターンしやすく Web フォーム ベースの ASP.NET Web アプリケーションをテストするよりも、アプリケーションをテストします。 たとえば、Web フォーム ベースの ASP.NET Web アプリケーションで 1 つのクラスは出力を表示して使用ユーザーの入力に応答します。 個々 のページをテストする必要がありますをインスタンス化する、ページ クラス、すべての子コントロール、およびアプリケーションに追加の依存クラスために、Web フォーム ベースの ASP.NET アプリケーションの自動テストの作成が複雑があります。 非常に多くのクラス、ページの実行をインスタンス化されているために、アプリケーションの個々 の部分に集中するテストを作成にくいことができます。 Web フォーム ベースの ASP.NET アプリケーションのテストは、MVC アプリケーションのテストよりも実装することが難しくしたがってできます。 さらに、Web フォーム ベースの ASP.NET アプリケーションでのテストでは、Web サーバーが必要です。 MVC フレームワークは、コンポーネントを分離し、フレームワークの残りの部分から分離環境で個々 のコンポーネントをテストできるように、インターフェイスが多用します。   
  
MVC アプリケーションは、次の 3 つの主要なコンポーネント間の疎結合では、並行開発も促進されます。 たとえば、1 人の開発者は、ビューで作業できる、2 番目の開発者、コント ローラー ロジックで作業でき、3 番目の開発者は、モデル内のビジネス ロジックに集中できます。

## <a name="deciding-when-to-create-an-mvc-application"></a>MVC アプリケーションを作成するかを決める

慎重に考慮する必要があります、ASP.NET MVC フレームワークまたは ASP.NET Web フォーム モデルのいずれかを使用して、Web アプリケーションを実装するかどうか。 MVC フレームワークは、Web フォーム モデルを置き換えられませんWeb アプリケーションのどちらのフレームワークを使用することができます。 (既存の Web フォーム ベースのアプリケーションがあれば、これら引き続きが常にあると同じように動作します。)   
  
特定の Web サイトの MVC フレームワークと Web フォーム モデルを使用する前に、各方法の利点を比較検討します。

### <a name="advantages-of-an-mvc-based-web-application"></a>MVC ベースの Web アプリケーションの利点

ASP.NET MVC フレームワークには次の利点があります。

- やすく、モデル、ビュー、およびコント ローラーにアプリケーションを分割して複雑さを管理します。
- これは、ビュー ステートやサーバー ベースのフォームには使用されません。 これは、MVC フレームワーク最適完全に制御するアプリケーションの動作をする開発者向けです。
- 単一のコント ローラーから Web アプリケーションの要求を処理するフロント コント ローラー パターンが使用されます。 これにより、さまざまなルーティング インフラストラクチャをサポートするアプリケーションを設計することができます。 詳細については、次を参照してください。[フロント コント ローラー](https://go.microsoft.com/fwlink/?LinkId=106357 "フロント コント ローラー") MSDN Web サイトです。
- テスト駆動開発 (TDD) のサポートの向上を提供します。
- 大規模なチームの開発者や、高度なアプリケーションの動作の制御を必要とする Web デザイナーでサポートされている Web アプリケーションに対して適切に機能します。

### <a name="advantages-of-a-web-forms-based-web-application"></a>Web フォーム ベースの Web アプリケーションの利点

Web フォーム ベースのフレームワークには次の利点があります。

- 基幹業務 Web アプリケーションの開発が HTTP 経由で状態を保持するイベント モデルがサポートしています。 Web フォーム ベースのアプリケーションでは、重複除去は数百台のサーバー コントロールでサポートされているイベントを提供します。
- 個々 のページに機能を追加するページ コント ローラー パターンが使用されます。 詳細については、次を参照してください。[ページ コント ローラー](https://go.microsoft.com/fwlink/?LinkId=106359 "ページ コント ローラー") MSDN Web サイトです。
- ビュー状態またはステータス情報の管理を容易にすることがある、サーバー ベースのフォームを使用します。
- Web 開発者と多数のアプリケーションの迅速な開発に使用できるコンポーネントを活用するために必要なデザイナーの小規模なチームに対して適切に機能します。
- 一般に、それほど複雑では、アプリケーション開発用ためコンポーネント (、**ページ**クラスやコントロールに) 緊密に統合されて、通常、MVC モデルよりも少ないコードを必要とします。

## <a name="features-of-the-aspnet-mvc-framework"></a>ASP.NET MVC フレームワークの機能

ASP.NET MVC フレームワークでは、次の機能を提供します。

- アプリケーション タスク (入力ロジック、ビジネス ロジック、および UI ロジック)、テストの容易性、および既定では、テスト駆動開発 (TDD) の分離。 MVC フレームワークのすべての主要なコントラクトはインターフェイス ベースであり、アプリケーションの実際のオブジェクトの動作を模倣するシミュレーション用のオブジェクトは、モック オブジェクトを使用してテストすることができます。 できる単体テストを行うアプリケーションに ASP.NET プロセス、単体テストの高速かつ柔軟で、コント ローラーを実行する必要はありません。 .NET Framework と互換性がある単体テスト フレームワークを使用できます。
- 拡張可能なプラグ可能なフレームワークです。 ASP.NET MVC フレームワークのコンポーネントでは、簡単に交換またはできるカスタマイズできるように設計されています。 独自のビュー エンジン、URL ルーティング ポリシー、アクション メソッド パラメーターのシリアル化、およびその他のコンポーネントに接続できます。 ASP.NET MVC フレームワークでは、依存関係の挿入 (DI) とコントロールの反転 (IOC) コンテナー モデルの使用もサポートします。 DI では、オブジェクト自体を作成するクラスではなく、クラスにオブジェクトを挿入することができます。 IOC は、あるオブジェクトは、別のオブジェクトを必要とする場合、オブジェクトを取得する 2 番目のオブジェクト、構成ファイルなどの外部ソースからを指定します。 これにより、テストが容易になります。
- アプリケーションと、わかりやすく検索可能な url を作成することができます、強力な URL マッピング コンポーネントです。 Url にファイル名拡張子を含める必要はありませんしいます検索に対して適切作業 URL の名前付けパターンをサポートするためにエンジンの最適化 (SEO) とアドレス指定 (REST) representational state transfer です。
- 既存の ASP.NET ページ (.aspx ファイル)、ユーザー コントロール (.ascx ファイル)、およびマスター ページ (.master ファイル) のマークアップ ファイル内のマークアップをビュー テンプレートとして使用するためのサポート。 できます入れ子になったマスター ページなど、ASP.NET MVC フレームワークで既存の ASP.NET 機能を使用して、インライン式 (&lt;% = %&gt;)、宣言型のサーバー コントロール、テンプレート、データ バインディング、ローカリゼーション、およびなどです。
- 既存の ASP.NET 機能をサポートします。 ASP.NET MVC では、フォーム認証と Windows 認証、URL 承認、メンバーシップとロール、出力とデータ キャッシュ、セッションとプロファイルの状態管理、正常性の監視、構成システムおよびプロバイダーなどの機能を使用できます。アーキテクチャです。