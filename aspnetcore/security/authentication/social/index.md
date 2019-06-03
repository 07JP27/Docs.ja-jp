---
title: Facebook、Google、ASP.NET Core での外部プロバイダーの認証
author: rick-anderson
description: このチュートリアルでは、OAuth 2.0 と外部の認証プロバイダーを使用して ASP.NET Core 2.x アプリを構築する方法について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2019
uid: security/authentication/social/index
ms.openlocfilehash: 8dac8a8a2276388414b6bb1211e970617b001637
ms.sourcegitcommit: ccbb84ae307a5bc527441d3d509c20b5c1edde05
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/19/2019
ms.locfileid: "65874809"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Facebook、Google、ASP.NET Core での外部プロバイダーの認証

作成者: [Valeriy Novytskyy](https://github.com/01binary)、[Rick Anderson](https://twitter.com/RickAndMSFT)

このチュートリアルでは、ユーザーが OAuth 2.0 と外部の認証プロバイダーの資格情報を使用してサインインできる、ASP.NET Core 2.2 アプリケーションを構築する方法について説明します。

ここでは、[Facebook](xref:security/authentication/facebook-logins)、[Twitter](xref:security/authentication/twitter-logins)、[Google](xref:security/authentication/google-logins)、および [Microsoft](xref:security/authentication/microsoft-logins) の各プロバイダーを対象に説明します。 他のプロバイダーは、[AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers)、[AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers) などのサードパーティ パッケージで利用できます。

![Facebook、Twitter、Google+、Windows のソーシャル メディア アイコン](index/_static/social.png)

既存の資格情報でユーザーがサインインできるようになると:
* ユーザーにとって便利です。
* サインイン プロセスの複雑な管理の多くが、サード パーティに移ります。 

ソーシャル ログインによってトラフィックとユーザーの変換を促進する方法の例については、[Facebook](https://www.facebook.com/unsupportedbrowser) と [Twitter](https://dev.twitter.com/resources/case-studies) によるケース スタディを参照してください。

## <a name="create-a-new-aspnet-core-project"></a>新しい .NET Core プロジェクトを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 新しいプロジェクトを作成します。
* **[ASP.NET Core Web アプリケーション]** 、 **[次へ]** の順に選択します。
* **[プロジェクト名]** を指定して、 **[場所]** を確認または変更します。 **[作成]** を選択します。
* ドロップダウンから **[ASP.NET Core 2.2]** を選択します。 テンプレートの一覧で **[Web アプリケーション]** を選択します。
* **[認証]** の下で、 **[変更]** を選択して認証を **[個人のユーザー アカウント]** に設定します。 **[OK]** を選択します。
* **[新しい ASP.NET Core Web アプリケーションを作成する]** ウィンドウで、 **[作成]** を選択します。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [統合ターミナル](https://code.visualstudio.com/docs/editor/integrated-terminal)を開きます。

* ディレクトリ (`cd`) を、プロジェクトを格納するフォルダーに変更します。

* 次のコマンドを実行します。

  ```console
  dotnet new webapp -o WebApp1 -au Individual -uld
  code -r WebApp1
  ```

  * `dotnet new` コマンドでは、*WebApp1* フォルダーに新しい Razor Pages プロジェクトが作成されます。
  * `-uld` では、SQLite ではなく LocalDB を使用します。 `-uld` を省略して SQLite を使用します。
  * `-au Individual` によって、個々の認証に対するコードを作成します。
  * `code` コマンドでは、Visual Studio Code の新しいインスタンス内に *WebApp1* フォルダーが開かれます。

* "**ビルドとデバッグに必要な資産が 'WebApp1' にありません。追加しますか?** " という内容のダイアログ ボックスが表示されたら、 **[はい]** を選択します。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[ファイル]** 、 **[新しいソリューション]** の順に選択します。
* サイドバーで **[.NET Core]**  >  **[アプリ]** の順に選択します。 **[Web アプリケーション]** テンプレートを選択します。 **[次へ]** を選択します。
* **[ターゲット フレームワーク]** ドロップダウンを **[.NET Core 2.2]** に設定します。 **[次へ]** を選択します。
* **[プロジェクト名]** を指定します。 **[場所]** を確認または変更します。 **[作成]** を選択します。

---

## <a name="apply-migrations"></a>移行を適用する

* アプリを実行し、 **[登録]** リンクを選択します。
* 新しいアカウントの電子メール アドレスとパスワードを入力し、 **[登録]** を選択します。
* 指示に従って移行を適用します。

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>SecretManager を使用して、ログイン プロバイダーから割り当てられたトークンを格納する

ソーシャル ログイン プロバイダーは、登録プロセス中に**アプリケーション ID** トークンと**アプリケーション シークレット** トークンを割り当てます。 完全なトークン名はプロバイダーにより異なります。 これらのトークンは、アプリが API にアクセスするために使用する資格情報を示します。 トークンは、[Secret Manager](xref:security/app-secrets#secret-manager) のヘルプにより、アプリの構成にリンクすることが可能な "シークレット" になります。 Secret Manager は、*appsettings.json* などの構成ファイルのトークンに格納される、セキュリティがさらに向上した方法です。

> [!IMPORTANT]
> Secret Manager は、開発目的のみのためのものです。 [Azure Key Vault 構成プロバイダー](xref:security/key-vault-configuration)により、Azure テストと運用のシークレットを格納し、保護することが可能です。

「[Safe storage of app secrets in development in ASP.NET Core](xref:security/app-secrets)」(ASP.NET Core での開発中にアプリのシークレットを安全に格納する) トピックの手順に従い、以下の各ログイン プロバイダーから割り当てられたトークンを格納できるようにします。

## <a name="setup-login-providers-required-by-your-application"></a>アプリケーションに必要なログイン プロバイダーをセットアップする

各プロバイダーを使用するようにアプリケーションを構成するには、以下のトピックを参照してください。

* [Facebook](xref:security/authentication/facebook-logins) の手順
* [Twitter](xref:security/authentication/twitter-logins) の手順
* [Google](xref:security/authentication/google-logins) の手順
* [Microsoft](xref:security/authentication/microsoft-logins) の手順
* [その他のプロバイダー](xref:security/authentication/otherlogins)の手順

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>必要に応じてパスワードを設定する

外部ログイン プロバイダーに登録するときに、アプリケーションにパスワードは登録していません。 そのため、サイトのパスワードを作成し、記憶する作業は軽減されますが、外部ログイン プロバイダーに依存することにもなります。 外部ログイン プロバイダーを使用できない場合、Web サイトにサインインすることができません。

外部プロバイダーでのサインイン プロセス中に設定した電子メール アドレスを使用して、パスワードを作成し、サインインするには、次の手順を実行します。

* 右上にある **[Hello &lt; 電子メール エイリアス&gt;]** リンクを選択して**管理**ビューに移動します。

![Web アプリケーションの管理ビュー](index/_static/pass1a.png)

* **[作成]** を選択します。

![パスワード ページを設定する](index/_static/pass2a.png)

* 有効なパスワードを設定します。そのパスワードと電子メール アドレスを使用してサインインできます。

## <a name="next-steps"></a>次の手順

* このキーの順に押します。では、外部認証プロバイダーを紹介し、外部ログインを ASP.NET Core アプリケーションに追加するために必要な前提条件について説明しました。

* アプリケーションに必要なプロバイダーのログインを構成するには、各プロバイダーのページを参照してください。

* ユーザーとそのアクセス トークン許可および更新トークンに関する追加のデータを保持することをお勧めします。 詳細については、「<xref:security/authentication/social/additional-claims>」を参照してください。
