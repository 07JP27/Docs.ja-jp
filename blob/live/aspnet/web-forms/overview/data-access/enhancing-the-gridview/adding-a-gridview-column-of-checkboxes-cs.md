---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
title: "チェック ボックス (c#) の GridView 列を追加する |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、チェック ボックスの列を G. の複数の行を選択した場合の直感的な方法をユーザーに提供する GridView コントロールに追加する方法、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2007
ms.topic: article
ms.assetid: f63a9443-2db0-4f80-8246-840d3e86c2a3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 4796d5d9fcf1f924e9baa9bc56424a9d719425c9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-gridview-column-of-checkboxes-c"></a><span data-ttu-id="4e1ca-103">チェック ボックス (c#) の GridView 列を追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-103">Adding a GridView Column of Checkboxes (C#)</span></span>
====================
<span data-ttu-id="4e1ca-104">によって[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="4e1ca-105">[サンプル アプリをダウンロード](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe)または[PDF のダウンロード](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-105">[Download Sample App](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_52_CS.exe) or [Download PDF](adding-a-gridview-column-of-checkboxes-cs/_static/datatutorial52cs1.pdf)</span></span>

> <span data-ttu-id="4e1ca-106">このチュートリアルを GridView の複数の行を選択した場合の直感的な方法をユーザーに提供する GridView コントロールのチェック ボックスの列を追加する方法を見ます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-106">This tutorial looks at how to add a column of check boxes to a GridView control to provide the user with an intuitive way of selecting multiple rows of the GridView.</span></span>


## <a name="introduction"></a><span data-ttu-id="4e1ca-107">はじめに</span><span class="sxs-lookup"><span data-stu-id="4e1ca-107">Introduction</span></span>

<span data-ttu-id="4e1ca-108">前のチュートリアルは、特定のレコードを選択するために GridView にラジオ ボタンの列を追加する方法を調査します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-108">In the preceding tutorial we examined how to add a column of radio buttons to the GridView for the purpose of selecting a particular record.</span></span> <span data-ttu-id="4e1ca-109">ラジオ ボタンの列は、ユーザーは、グリッドから最大で 1 つの項目を選択するのには限定ときに適切なユーザー インターフェイスです。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-109">A column of radio buttons is a suitable user interface when the user is limited to choosing at most one item from the grid.</span></span> <span data-ttu-id="4e1ca-110">ときに、ただし、することも、グリッドからのアイテムの任意の数を取得するユーザーを許可します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-110">At times, however, we may want to allow the user to pick an arbitrary number of items from the grid.</span></span> <span data-ttu-id="4e1ca-111">Web ベースの電子メール クライアント、たとえば、通常表示チェック ボックスの列を含むメッセージの一覧です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-111">Web-based email clients, for example, typically display the list of messages with a column of checkboxes.</span></span> <span data-ttu-id="4e1ca-112">ユーザーは、メッセージの任意の数を選択し、電子メールを別のフォルダーに移動または削除を行うなど、何らかのアクションを実行できます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-112">The user can select an arbitrary number of messages and then perform some action, such as moving the emails to another folder or deleting them.</span></span>

<span data-ttu-id="4e1ca-113">このチュートリアルではチェック ボックスの列を追加する方法とポストバックにチェックインされたどのようなチェック ボックスを確認する方法が表示されます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-113">In this tutorial we will see how to add a column of checkboxes and how to determine what checkboxes were checked on postback.</span></span> <span data-ttu-id="4e1ca-114">具体的には、web ベースの電子メール クライアントのユーザー インターフェイスを正確に模倣する例をビルドします。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-114">In particular, we'll build an example that closely mimics the web-based email client user interface.</span></span> <span data-ttu-id="4e1ca-115">この例では、製品を一覧表示するページの GridView が含まれます、`Products`各チェック ボックスを持つデータベース テーブルの行 (図 1 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-115">Our example will include a paged GridView listing the products in the `Products` database table with a checkbox in each row (see Figure 1).</span></span> <span data-ttu-id="4e1ca-116">選択した製品の削除ボタンをクリックすると、選択したこれらの製品が削除されます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-116">A Delete Selected Products button, when clicked, will delete those products selected.</span></span>


<span data-ttu-id="4e1ca-117">[![各製品の行には、チェック ボックスが含まれています。](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-117">[![Each Product Row Includes a Checkbox](adding-a-gridview-column-of-checkboxes-cs/_static/image1.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="4e1ca-118">**図 1**: 各製品の行には、チェック ボックスが含まれています ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-118">**Figure 1**: Each Product Row Includes a Checkbox ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image2.png))</span></span>


## <a name="step-1-adding-a-paged-gridview-that-lists-product-information"></a><span data-ttu-id="4e1ca-119">手順 1: 製品情報を一覧表示するページ GridView の追加</span><span class="sxs-lookup"><span data-stu-id="4e1ca-119">Step 1: Adding a Paged GridView that Lists Product Information</span></span>

<span data-ttu-id="4e1ca-120">チェック ボックスの列を追加する方法を気にし、前にページングをサポートする GridView では、製品の一覧に最初注目を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-120">Before we worry about adding a column of checkboxes, let s first focus on listing the products in a GridView that supports paging.</span></span> <span data-ttu-id="4e1ca-121">開いて開始、 `CheckBoxField.aspx`  ページで、`EnhancedGridView`フォルダーと、デザイナーの設定には、ツールボックスからドラッグ、GridView、`ID`に`Products`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-121">Start by opening the `CheckBoxField.aspx` page in the `EnhancedGridView` folder and drag a GridView from the Toolbox onto the Designer, setting its `ID` to `Products`.</span></span> <span data-ttu-id="4e1ca-122">次に、という名前の新しい ObjectDataSource、GridView にバインドする選択`ProductsDataSource`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-122">Next, choose to bind the GridView to a new ObjectDataSource named `ProductsDataSource`.</span></span> <span data-ttu-id="4e1ca-123">構成を使用する ObjectDataSource、`ProductsBLL`クラス、呼び出し、`GetProducts()`データを返すメソッド。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-123">Configure the ObjectDataSource to use the `ProductsBLL` class, calling the `GetProducts()` method to return the data.</span></span> <span data-ttu-id="4e1ca-124">この GridView は読み取り専用になる、ため、更新、挿入でドロップダウン リストを設定し、(なし) にタブを削除します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-124">Since this GridView will be read-only, set the drop-down lists in the UPDATE, INSERT, and DELETE tabs to (None) .</span></span>


<span data-ttu-id="4e1ca-125">[![ProductsDataSource をという名前の新しい ObjectDataSource を作成します。](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-125">[![Create a New ObjectDataSource Named ProductsDataSource](adding-a-gridview-column-of-checkboxes-cs/_static/image2.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image3.png)</span></span>

<span data-ttu-id="4e1ca-126">**図 2**: 名前付き新しい ObjectDataSource 作成`ProductsDataSource`([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-126">**Figure 2**: Create a New ObjectDataSource Named `ProductsDataSource` ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image4.png))</span></span>


<span data-ttu-id="4e1ca-127">[![構成、ObjectDataSource GetProducts() メソッドを使用してデータを取得するには](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-127">[![Configure the ObjectDataSource to Retrieve Data Using the GetProducts() Method](adding-a-gridview-column-of-checkboxes-cs/_static/image3.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image5.png)</span></span>

<span data-ttu-id="4e1ca-128">**図 3**: 構成を使用してデータを取得する ObjectDataSource、`GetProducts()`メソッド ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-128">**Figure 3**: Configure the ObjectDataSource to Retrieve Data Using the `GetProducts()` Method ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image6.png))</span></span>


<span data-ttu-id="4e1ca-129">[![UPDATE、INSERT のドロップダウン リストを設定し、(なし) タブを削除します。](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-129">[![Set the Drop-Down Lists in the UPDATE, INSERT, and DELETE Tabs to (None)](adding-a-gridview-column-of-checkboxes-cs/_static/image4.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image7.png)</span></span>

<span data-ttu-id="4e1ca-130">**図 4**: (なし) に、UPDATE、INSERT、および削除のタブで、ドロップダウン リストを設定 ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-130">**Figure 4**: Set the Drop-Down Lists in the UPDATE, INSERT, and DELETE Tabs to (None) ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image8.png))</span></span>


<span data-ttu-id="4e1ca-131">データ ソース構成ウィザードを完了すると、Visual Studio は BoundColumns および製品に関連するデータ フィールドの CheckBoxColumn 自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-131">After completing the Configure Data Source wizard, Visual Studio will automatically create BoundColumns and a CheckBoxColumn for the product-related data fields.</span></span> <span data-ttu-id="4e1ca-132">前のチュートリアルで行ったように、削除以外のすべての`ProductName`、 `CategoryName`、および`UnitPrice`BoundFields、し、変更、`HeaderText`製品カテゴリ、および価格プロパティです。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-132">Like we did in the previous tutorial, remove all but the `ProductName`, `CategoryName`, and `UnitPrice` BoundFields, and change the `HeaderText` properties to Product, Category, and Price.</span></span> <span data-ttu-id="4e1ca-133">構成、 `UnitPrice` BoundField その値が通貨として書式設定されるようにします。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-133">Configure the `UnitPrice` BoundField so that its value is formatted as a currency.</span></span> <span data-ttu-id="4e1ca-134">また、スマート タグからページングを有効にする チェック ボックスをチェックしてページングをサポートする GridView を構成します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-134">Also configure the GridView to support paging by checking the Enable Paging checkbox from the smart tag.</span></span>

<span data-ttu-id="4e1ca-135">追加のユーザー インターフェイスを選択した製品を削除することも s を使用できます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-135">Let s also add the user interface for deleting the selected products.</span></span> <span data-ttu-id="4e1ca-136">設定、GridView の下にあるボタン Web コントロールを追加、`ID`に`DeleteSelectedProducts`とその`Text`プロパティを選択した製品を削除します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-136">Add a Button Web control beneath the GridView, setting its `ID` to `DeleteSelectedProducts` and its `Text` property to Delete Selected Products.</span></span> <span data-ttu-id="4e1ca-137">データベースから製品を実際に削除するではなくこの例でおだけを表示、削除された製品を示すメッセージがします。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-137">Rather than actually deleting products from the database, for this example we'll just display a message stating the products that would have been deleted.</span></span> <span data-ttu-id="4e1ca-138">これに合わせて、ボタンの下にラベル Web コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-138">To accommodate this, add a Label Web control beneath the Button.</span></span> <span data-ttu-id="4e1ca-139">ID を設定`DeleteResults`をクリア、その`Text`プロパティ、およびセットその`Visible`と`EnableViewState`プロパティ`false`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-139">Set its ID to `DeleteResults`, clear out its `Text` property, and set its `Visible` and `EnableViewState` properties to `false`.</span></span>

<span data-ttu-id="4e1ca-140">これらの変更を加えたら、GridView、ObjectDataSource、ボタン、およびラベルは宣言型マークアップを次のように行ってください。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-140">After making these changes, the GridView, ObjectDataSource, Button, and Label s declarative markup should similar to the following:</span></span>


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="4e1ca-141">ブラウザーでページを表示する (図 5 を参照してください)。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-141">Take a moment to view the page in a browser (see Figure 5).</span></span> <span data-ttu-id="4e1ca-142">この時点で名前、カテゴリ、および最初の 10 個の製品の価格を参照する必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-142">At this point you should see the name, category, and price of the first ten products.</span></span>


<span data-ttu-id="4e1ca-143">[![名前、カテゴリ、および最初の 10 個の製品の価格の一覧が表示されます。](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-143">[![The Name, Category, and Price of the First Ten Products are Listed](adding-a-gridview-column-of-checkboxes-cs/_static/image5.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image9.png)</span></span>

<span data-ttu-id="4e1ca-144">**図 5**: Name、カテゴリ、および最初の 10 個の製品の価格の一覧が表示されます ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-144">**Figure 5**: The Name, Category, and Price of the First Ten Products are Listed ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image10.png))</span></span>


## <a name="step-2-adding-a-column-of-checkboxes"></a><span data-ttu-id="4e1ca-145">手順 2: チェック ボックスの列を追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-145">Step 2: Adding a Column of Checkboxes</span></span>

<span data-ttu-id="4e1ca-146">ASP.NET 2.0 には、CheckBoxField が含まれているために使用 GridView にチェック ボックスの列を追加することが考えることができます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-146">Since ASP.NET 2.0 includes a CheckBoxField, one might think that it could be used to add a column of checkboxes to a GridView.</span></span> <span data-ttu-id="4e1ca-147">残念ながら、れていない場合は、CheckBoxField がブール型のデータ フィールドを使用するために設計されました。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-147">Unfortunately, that is not the case, as the CheckBoxField is designed to work with a Boolean data field.</span></span> <span data-ttu-id="4e1ca-148">つまり、CheckBoxField を使用するために基になるデータ フィールドが表示されたチェック ボックスがオンになっているかどうかを決定する値を持つを検討する必要があります指定します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-148">That is, in order to use the CheckBoxField we must specify the underlying data field whose value is consulted to determine whether the rendered checkbox is checked.</span></span> <span data-ttu-id="4e1ca-149">CheckBoxField のチェック ボックスをオンになっていない列を含めるだけを使用できません。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-149">We cannot use the CheckBoxField to just include a column of unchecked checkboxes.</span></span>

<span data-ttu-id="4e1ca-150">代わりに、TemplateField を追加し、するチェック ボックスをオン Web コントロールを追加する必要があります、`ItemTemplate`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-150">Instead, we must add a TemplateField and add a CheckBox Web control to its `ItemTemplate`.</span></span> <span data-ttu-id="4e1ca-151">TemplateField を追加してください、 `Products` GridView され、最初の (左端まで) フィールドになります。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-151">Go ahead and add a TemplateField to the `Products` GridView and make it the first (far-left) field.</span></span> <span data-ttu-id="4e1ca-152">GridView s のスマート タグからのテンプレートの編集リンクをクリックし、ツールボックスから、チェック ボックスをオン Web コントロールをドラッグ、`ItemTemplate`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-152">From the GridView s smart tag, click on the Edit Templates link and then drag a CheckBox Web control from the Toolbox into the `ItemTemplate`.</span></span> <span data-ttu-id="4e1ca-153">このチェック ボックスをオン s 設定`ID`プロパティを`ProductSelector`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-153">Set this CheckBox s `ID` property to `ProductSelector`.</span></span>


<span data-ttu-id="4e1ca-154">[![TemplateField の ItemTemplate を ProductSelector をという名前のチェック ボックスをオン Web コントロールを追加します。](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-154">[![Add a CheckBox Web Control Named ProductSelector to the TemplateField s ItemTemplate](adding-a-gridview-column-of-checkboxes-cs/_static/image6.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image11.png)</span></span>

<span data-ttu-id="4e1ca-155">**図 6**: という名前 チェック ボックス Web コントロールを追加`ProductSelector`TemplateField s `ItemTemplate` ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-155">**Figure 6**: Add a CheckBox Web Control Named `ProductSelector` to the TemplateField s `ItemTemplate` ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image12.png))</span></span>


<span data-ttu-id="4e1ca-156">各行には TemplateField CheckBox Web コントロールに追加された、チェック ボックスが含まれています。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-156">With the TemplateField and CheckBox Web control added, each row now includes a checkbox.</span></span> <span data-ttu-id="4e1ca-157">図 7 は、TemplateField とチェック ボックスを追加した後、ブラウザーで表示したときに、このページを示します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-157">Figure 7 shows this page, when viewed through a browser, after the TemplateField and CheckBox have been added.</span></span>


<span data-ttu-id="4e1ca-158">[![各製品の行には、チェック ボックスが含まれています](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-158">[![Each Product Row Now Includes a Checkbox](adding-a-gridview-column-of-checkboxes-cs/_static/image7.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image13.png)</span></span>

<span data-ttu-id="4e1ca-159">**図 7**: 各製品の行には、チェック ボックスが含まれています ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-159">**Figure 7**: Each Product Row Now Includes a Checkbox ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image14.png))</span></span>


## <a name="step-3-determining-what-checkboxes-were-checked-on-postback"></a><span data-ttu-id="4e1ca-160">手順 3: はポストバックでチェックされたどのようなチェック ボックスを決定します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-160">Step 3: Determining What Checkboxes Were Checked On Postback</span></span>

<span data-ttu-id="4e1ca-161">この時点で、列のチェック ボックスがどのようなチェック ボックスがポストバック時にチェックを決定する方法がありませんがあります。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-161">At this point we have a column of checkboxes but no way to determine what checkboxes were checked on postback.</span></span> <span data-ttu-id="4e1ca-162">ただし、選択した製品の削除 ボタンがクリックされたときに、これらの製品を削除するのにはどのようなチェック ボックスをオンにチェックインされたおく必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-162">When the Delete Selected Products button is clicked, though, we need to know what checkboxes were checked in order to delete those products.</span></span>

<span data-ttu-id="4e1ca-163">GridView s [ `Rows`プロパティ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx)GridView 内のデータ行へのアクセスを提供します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-163">The GridView s [`Rows` property](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview.rows.aspx) provides access to the data rows in the GridView.</span></span> <span data-ttu-id="4e1ca-164">これらの行を反復処理できる CheckBox コントロールをプログラムでアクセスし、しを参照してください、`Checked`チェック ボックスが選択されているかどうかを決定するプロパティです。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-164">We can iterate through these rows, programmatically access the CheckBox control, and then consult its `Checked` property to determine whether the CheckBox has been selected.</span></span>

<span data-ttu-id="4e1ca-165">イベント ハンドラーを作成、`DeleteSelectedProducts`ボタン Web コントロールの`Click`イベントし、次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-165">Create an event handler for the `DeleteSelectedProducts` Button Web control s `Click` event and add the following code:</span></span>


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample2.cs)]

<span data-ttu-id="4e1ca-166">`Rows`プロパティのコレクションを返します`GridViewRow`GridView のデータ行をインスタンス化します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-166">The `Rows` property returns a collection of `GridViewRow` instances that makeup the GridView s data rows.</span></span> <span data-ttu-id="4e1ca-167">`foreach`ループをここでは、このコレクションを列挙します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-167">The `foreach` loop here enumerates this collection.</span></span> <span data-ttu-id="4e1ca-168">各`GridViewRow`オブジェクト、行のチェック ボックスを使用してプログラムでアクセス`row.FindControl("controlID")`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-168">For each `GridViewRow` object, the row s CheckBox is programmatically accessed using `row.FindControl("controlID")`.</span></span> <span data-ttu-id="4e1ca-169">チェック ボックスをオンにした場合、秒の行の対応する`ProductID`から値を取得、`DataKeys`コレクション。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-169">If the CheckBox is checked, the row s corresponding `ProductID` value is retrieved from the `DataKeys` collection.</span></span> <span data-ttu-id="4e1ca-170">この演習では単に情報メッセージを表示で、`DeleteResults`ラベル付け、実用的なアプリケーションで d は代わりにも作成への呼び出し、`ProductsBLL`クラスの`DeleteProduct(productID)`メソッドです。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-170">In this exercise, we simply display an informative message in the `DeleteResults` Label, although in a working application we d instead make a call to the `ProductsBLL` class s `DeleteProduct(productID)` method.</span></span>

<span data-ttu-id="4e1ca-171">このイベント ハンドラーの追加により、今すぐ選択した製品の削除 ボタンをクリックすると表示、`ProductID`選択した製品の秒。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-171">With the addition of this event handler, clicking the Delete Selected Products button now displays the `ProductID` s of the selected products.</span></span>


<span data-ttu-id="4e1ca-172">[![選択されている製品の削除ボタンがクリックされたときに、選択した製品 Productid が一覧表示されます。](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-172">[![When the Delete Selected Products Button is Clicked the Selected Products ProductIDs are Listed](adding-a-gridview-column-of-checkboxes-cs/_static/image8.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image15.png)</span></span>

<span data-ttu-id="4e1ca-173">**図 8**: ときに、削除選択されている製品ボタンがクリックされた選択した製品`ProductID`s が一覧表示されます ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-173">**Figure 8**: When the Delete Selected Products Button is Clicked the Selected Products `ProductID` s are Listed ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image16.png))</span></span>


## <a name="step-4-adding-check-all-and-uncheck-all-buttons"></a><span data-ttu-id="4e1ca-174">手順 4: 追加のすべてをチェックおよびすべてのボタンをオフに</span><span class="sxs-lookup"><span data-stu-id="4e1ca-174">Step 4: Adding Check All and Uncheck All Buttons</span></span>

<span data-ttu-id="4e1ca-175">場合は、ユーザーが現在のページ上のすべての製品を削除しようとすると、10 個のチェック ボックスの各チェックする必要があります。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-175">If a user wants to delete all products on the current page, they must check each of the ten checkboxes.</span></span> <span data-ttu-id="4e1ca-176">すべてを確認して追加することでこのプロセスを高速化を支援ボタンをクリックすると、グリッド内のすべてのチェック ボックスの選択します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-176">We can help expedite this process by adding a Check All button that, when clicked, selects all of the checkboxes in the grid.</span></span> <span data-ttu-id="4e1ca-177">オフにしてすべてのボタンが均等に役に立ちます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-177">An Uncheck All button would be equally helpful.</span></span>

<span data-ttu-id="4e1ca-178">GridView 上に配置すること、ページには、2 つのボタンの Web コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-178">Add two Button Web controls to the page, placing them above the GridView.</span></span> <span data-ttu-id="4e1ca-179">最初の 1 つの s を設定`ID`に`CheckAll`とその`Text`すべてをチェックするプロパティ以外のセット、2 つ目の 1 つの s`ID`に`UncheckAll`とその`Text`プロパティをすべてオフにします。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-179">Set the first one s `ID` to `CheckAll` and its `Text` property to Check All ; set the second one s `ID` to `UncheckAll` and its `Text` property to Uncheck All .</span></span>


[!code-aspx[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="4e1ca-180">という名前の分離コード クラスにメソッドを次に、作成`ToggleCheckState(checkState)`、呼び出されると、列挙、 `Products` GridView s`Rows`コレクション設定の各チェック ボックスをオン s および`Checked`プロパティを渡されたの値にで*checkState*パラメーター。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-180">Next, create a method in the code-behind class named `ToggleCheckState(checkState)` that, when invoked, enumerates the `Products` GridView s `Rows` collection and sets each CheckBox s `Checked` property to the value of the passed in *checkState* parameter.</span></span>


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample4.cs)]

<span data-ttu-id="4e1ca-181">次に、作成`Click`のイベント ハンドラー、`CheckAll`と`UncheckAll`ボタン。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-181">Next, create `Click` event handlers for the `CheckAll` and `UncheckAll` buttons.</span></span> <span data-ttu-id="4e1ca-182">`CheckAll` S イベント ハンドラー、単に呼び出し`ToggleCheckState(true)` `UncheckAll`、呼び出す`ToggleCheckState(false)`です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-182">In `CheckAll` s event handler, simply call `ToggleCheckState(true)`; in `UncheckAll`, call `ToggleCheckState(false)`.</span></span>


[!code-csharp[Main](adding-a-gridview-column-of-checkboxes-cs/samples/sample5.cs)]

<span data-ttu-id="4e1ca-183">このコードは、すべてのボタンをクリックするとポストバックを発生させるし、すべて GridView で対応するチェック ボックスのチェックします。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-183">With this code, clicking the Check All button causes a postback and checks all of the checkboxes in the GridView.</span></span> <span data-ttu-id="4e1ca-184">同様に、すべてをオフにしてをクリックすると、すべてのチェック ボックスが選択解除します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-184">Likewise, clicking Uncheck All unselects all checkboxes.</span></span> <span data-ttu-id="4e1ca-185">図 9 は、すべてのボタンがチェックされた後、画面を示しています。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-185">Figure 9 shows the screen after the Check All button has been checked.</span></span>


<span data-ttu-id="4e1ca-186">[![[すべて] ボタン、チェックをクリックすると、すべてのチェック ボックスを選択します。](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-186">[![Clicking the Check All Button Selects All Checkboxes](adding-a-gridview-column-of-checkboxes-cs/_static/image9.gif)](adding-a-gridview-column-of-checkboxes-cs/_static/image17.png)</span></span>

<span data-ttu-id="4e1ca-187">**図 9**: チェックすべてボタンを選択すべてチェック ボックスをクリックすると ([フルサイズのイメージを表示するをクリックして](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="4e1ca-187">**Figure 9**: Clicking the Check All Button Selects All Checkboxes ([Click to view full-size image](adding-a-gridview-column-of-checkboxes-cs/_static/image18.png))</span></span>


> [!NOTE]
> <span data-ttu-id="4e1ca-188">ときに、チェック ボックスをオンまたはオフにすると、対応するチェック ボックスのすべての方法の 1 つの列を表示するには、ヘッダー行のチェック ボックスです。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-188">When displaying a column of checkboxes, one approach for selecting or unselecting all of the checkboxes is through a checkbox in the header row.</span></span> <span data-ttu-id="4e1ca-189">さらに、すべて現在チェック/オフにしてすべての実装には、ポストバックが必要です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-189">Moreover, the current Check All / Uncheck All implementation requires a postback.</span></span> <span data-ttu-id="4e1ca-190">対応するチェック ボックス使用できる checked または unchecked、ただし、完全 ~ クライアント側-スクリプト、良くユーザー エクスペリエンスを提供します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-190">The checkboxes can be checked or unchecked, however, entirely through client-side script, thereby providing a snappier user experience.</span></span> <span data-ttu-id="4e1ca-191">探索と共にクライアント側の技法の使用についての詳細ですべてをチェックし、すべてオフのヘッダー行のチェック ボックスを使用するのにはチェック アウト[チェックすべてのチェック ボックスで、クライアント側スクリプトを使用して GridView と、すべてチェック ボックスの確認](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx)です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-191">To explore using a header row checkbox for Check All and Uncheck All in detail, along with a discussion on using client-side techniques, check out [Checking All CheckBoxes in a GridView Using Client-Side Script and a Check All CheckBox](http://aspnet.4guysfromrolla.com/articles/053106-1.aspx).</span></span>


## <a name="summary"></a><span data-ttu-id="4e1ca-192">概要</span><span class="sxs-lookup"><span data-stu-id="4e1ca-192">Summary</span></span>

<span data-ttu-id="4e1ca-193">行の任意の数を続行する前に GridView から選択できるようにする必要がある場合、1 つのオプションは、チェック ボックスの列を追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-193">In cases where you need to let users choose an arbitrary number of rows from a GridView before proceeding, adding a column of checkboxes is one option.</span></span> <span data-ttu-id="4e1ca-194">このチュートリアルで説明したとおり、GridView のチェック ボックスの列を含むだけで、CheckBox Web コントロールで TemplateField を追加します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-194">As we saw in this tutorial, including a column of checkboxes in the GridView entails adding a TemplateField with a CheckBox Web control.</span></span> <span data-ttu-id="4e1ca-195">(前のチュートリアルで行ったようには、マークアップ、テンプレートに直接挿入する) と Web コントロールを使用して、ASP.NET は、何のチェック ボックス、ポストバック間ではチェックされませんでしたに自動的に保存します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-195">By using a Web control (versus injecting markup directly into the template, as we did in the previous tutorial) ASP.NET automatically remembers what CheckBoxes were and were not checked across postback.</span></span> <span data-ttu-id="4e1ca-196">対応するチェック ボックスのコードで確認するのに指定されたチェック ボックスを確認するかどうかチェック状態を変更するプログラムからもアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-196">We can also programmatically access the CheckBoxes in code to determine whether a given CheckBox is checked, or to chnage the checked state.</span></span>

<span data-ttu-id="4e1ca-197">このチュートリアルと最後の 1 つは、GridView に、行セレクターの列を追加することで検索します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-197">This tutorial and the last one looked at adding a row selector column to the GridView.</span></span> <span data-ttu-id="4e1ca-198">[次へ]、チュートリアルでは、作業のビット、お機能を追加できます挿入 GridView にについて確認します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-198">In our next tutorial we'll examine how, with a bit of work, we can add inserting capabilities to the GridView.</span></span>

<span data-ttu-id="4e1ca-199">満足プログラミング!</span><span class="sxs-lookup"><span data-stu-id="4e1ca-199">Happy Programming!</span></span>

## <a name="about-the-author"></a><span data-ttu-id="4e1ca-200">作成者について</span><span class="sxs-lookup"><span data-stu-id="4e1ca-200">About the Author</span></span>

<span data-ttu-id="4e1ca-201">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、7 つ受け取りますブックとの創設者の作成者[4GuysFromRolla.com](http://www.4guysfromrolla.com)、1998 年からマイクロソフトの Web テクノロジで取り組んできました。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-201">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of seven ASP/ASP.NET books and founder of [4GuysFromRolla.com](http://www.4guysfromrolla.com), has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="4e1ca-202">Scott は、コンサルタント、トレーナー、ライターとして機能します。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-202">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="4e1ca-203">最新の著書[ *Sam 学べる自分で ASP.NET 2.0 が 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-203">His latest book is [*Sams Teach Yourself ASP.NET 2.0 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco).</span></span> <span data-ttu-id="4e1ca-204">彼に到達できる[ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)彼のブログを使用して含まれているのか[http://ScottOnWriting.NET](http://ScottOnWriting.NET)です。</span><span class="sxs-lookup"><span data-stu-id="4e1ca-204">He can be reached at [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) or via his blog, which can be found at [http://ScottOnWriting.NET](http://ScottOnWriting.NET).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="4e1ca-205">[前へ](adding-a-gridview-column-of-radio-buttons-cs.md)
[次へ](inserting-a-new-record-from-the-gridview-s-footer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="4e1ca-205">[Previous](adding-a-gridview-column-of-radio-buttons-cs.md)
[Next](inserting-a-new-record-from-the-gridview-s-footer-cs.md)</span></span>