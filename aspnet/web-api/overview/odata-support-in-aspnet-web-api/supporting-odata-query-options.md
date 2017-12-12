---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "ASP.NET Web API 2 OData クエリ オプションをサポートする |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="fceec-102">ASP.NET web API 2 OData クエリ オプションをサポートします。</span><span class="sxs-lookup"><span data-stu-id="fceec-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="fceec-103">によって[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="fceec-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="fceec-104">OData は、OData のクエリを変更するために使用するパラメーターを定義します。</span><span class="sxs-lookup"><span data-stu-id="fceec-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="fceec-105">クライアントは、要求 URI のクエリ文字列内のこれらのパラメーターを送信します。</span><span class="sxs-lookup"><span data-stu-id="fceec-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="fceec-106">たとえば、クライアントに結果を並べ替えるには、$orderby パラメーターを使用してください。</span><span class="sxs-lookup"><span data-stu-id="fceec-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="fceec-107">OData の仕様は、これらのパラメーターを呼び出す*クエリ オプション*です。</span><span class="sxs-lookup"><span data-stu-id="fceec-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="fceec-108">プロジェクト &#8212; で任意の Web API コント ローラーの OData クエリ オプションを有効にすることができます。コント ローラーは、OData エンドポイントである必要はありません。</span><span class="sxs-lookup"><span data-stu-id="fceec-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="fceec-109">これにより、フィルター処理および並べ替えを任意の Web API アプリケーションなどの機能を追加する便利な方法です。</span><span class="sxs-lookup"><span data-stu-id="fceec-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="fceec-110">トピックを参照してください。 クエリのオプションを有効にする前に[OData セキュリティ ガイダンス](odata-security-guidance.md)です。</span><span class="sxs-lookup"><span data-stu-id="fceec-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="fceec-111">OData クエリ オプションを有効にします。</span><span class="sxs-lookup"><span data-stu-id="fceec-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="fceec-112">クエリの例</span><span class="sxs-lookup"><span data-stu-id="fceec-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="fceec-113">サーバー ドリブン ページング</span><span class="sxs-lookup"><span data-stu-id="fceec-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="fceec-114">クエリ オプションを制限します。</span><span class="sxs-lookup"><span data-stu-id="fceec-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="fceec-115">クエリ オプションの直接呼び出し</span><span class="sxs-lookup"><span data-stu-id="fceec-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="fceec-116">クエリの検証</span><span class="sxs-lookup"><span data-stu-id="fceec-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="fceec-117">OData クエリ オプションを有効にします。</span><span class="sxs-lookup"><span data-stu-id="fceec-117">Enabling OData Query Options</span></span>

<span data-ttu-id="fceec-118">Web API には、次の OData クエリ オプションがサポートされています。</span><span class="sxs-lookup"><span data-stu-id="fceec-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="fceec-119">オプション</span><span class="sxs-lookup"><span data-stu-id="fceec-119">Option</span></span> | <span data-ttu-id="fceec-120">説明</span><span class="sxs-lookup"><span data-stu-id="fceec-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="fceec-121">$ を展開します。</span><span class="sxs-lookup"><span data-stu-id="fceec-121">$expand</span></span> | <span data-ttu-id="fceec-122">関連エンティティのインライン展開されます。</span><span class="sxs-lookup"><span data-stu-id="fceec-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="fceec-123">$filter</span><span class="sxs-lookup"><span data-stu-id="fceec-123">$filter</span></span> | <span data-ttu-id="fceec-124">ブール条件に基づいて、結果をフィルター処理します。</span><span class="sxs-lookup"><span data-stu-id="fceec-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="fceec-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="fceec-125">$inlinecount</span></span> | <span data-ttu-id="fceec-126">応答に対応するエンティティの合計数を追加するには、サーバーに指示します。</span><span class="sxs-lookup"><span data-stu-id="fceec-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="fceec-127">(サーバー側のページングに役立ちます。)</span><span class="sxs-lookup"><span data-stu-id="fceec-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="fceec-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="fceec-128">$orderby</span></span> | <span data-ttu-id="fceec-129">結果を並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="fceec-129">Sorts the results.</span></span> |
| <span data-ttu-id="fceec-130">$select</span><span class="sxs-lookup"><span data-stu-id="fceec-130">$select</span></span> | <span data-ttu-id="fceec-131">応答に含めるプロパティを選択します。</span><span class="sxs-lookup"><span data-stu-id="fceec-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="fceec-132">$skip</span><span class="sxs-lookup"><span data-stu-id="fceec-132">$skip</span></span> | <span data-ttu-id="fceec-133">最初の n 個の結果をスキップします。</span><span class="sxs-lookup"><span data-stu-id="fceec-133">Skips the first n results.</span></span> |
| <span data-ttu-id="fceec-134">$top</span><span class="sxs-lookup"><span data-stu-id="fceec-134">$top</span></span> | <span data-ttu-id="fceec-135">最初の n のみ結果を返します。</span><span class="sxs-lookup"><span data-stu-id="fceec-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="fceec-136">OData クエリ オプションを使用するには、必要があります有効にするに明示的にします。</span><span class="sxs-lookup"><span data-stu-id="fceec-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="fceec-137">全体のアプリケーションに対してグローバルに有効にしたり、特定のコント ローラーまたは特定のアクションを有効にすることできます。</span><span class="sxs-lookup"><span data-stu-id="fceec-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="fceec-138">OData クエリ オプションをグローバルに有効にするを呼び出す**EnableQuerySupport**上、 **HttpConfiguration**起動時クラス。</span><span class="sxs-lookup"><span data-stu-id="fceec-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="fceec-139">**EnableQuerySupport**メソッドを返すコント ローラーのアクションのグローバル クエリ オプションを有効にする**IQueryable**型です。</span><span class="sxs-lookup"><span data-stu-id="fceec-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="fceec-140">クエリ オプションをアプリケーション全体に対して有効にしない場合を有効にすることの特定のコント ローラー アクションの追加、 **[Queryable]**属性をアクション メソッドにします。</span><span class="sxs-lookup"><span data-stu-id="fceec-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="fceec-141">クエリの例</span><span class="sxs-lookup"><span data-stu-id="fceec-141">Example Queries</span></span>

<span data-ttu-id="fceec-142">このセクションでは、OData クエリ オプションを使用してクエリの種類を示します。</span><span class="sxs-lookup"><span data-stu-id="fceec-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="fceec-143">特定の詳細については、クエリ オプションでは、ドキュメントを参照して、OData に[www.odata.org](http://www.odata.org/)です。</span><span class="sxs-lookup"><span data-stu-id="fceec-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="fceec-144">$ について順に展開し、$select を参照してください[$ $select を使用すると、展開しとで ASP.NET Web API OData $value](using-select-expand-and-value.md)です。</span><span class="sxs-lookup"><span data-stu-id="fceec-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="fceec-145">**クライアント主導のページング**</span><span class="sxs-lookup"><span data-stu-id="fceec-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="fceec-146">大きなエンティティ セットに対して、クライアントで結果の数を制限する必要が場合があります。</span><span class="sxs-lookup"><span data-stu-id="fceec-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="fceec-147">など、クライアントでは、結果の次のページを取得する [次へ] のリンクと共に、一度に 10 個のエントリを表示する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="fceec-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="fceec-148">これを行うには、クライアントは、$top、$skip のオプションを使用します。</span><span class="sxs-lookup"><span data-stu-id="fceec-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="fceec-149">$Top オプションが返される項目の最大数を提供し、$skip オプションは、スキップするエントリの数を提供します。</span><span class="sxs-lookup"><span data-stu-id="fceec-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="fceec-150">前の例では、21 30 からのエントリをフェッチします。</span><span class="sxs-lookup"><span data-stu-id="fceec-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="fceec-151">**フィルター処理**</span><span class="sxs-lookup"><span data-stu-id="fceec-151">**Filtering**</span></span>

<span data-ttu-id="fceec-152">$Filter オプションは、ブール式を適用することにより、結果をフィルター処理クライアントを使用します。</span><span class="sxs-lookup"><span data-stu-id="fceec-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="fceec-153">フィルター式では、非常に強力です。論理演算子および算術演算子、文字列関数、および日付関数が含まれます。</span><span class="sxs-lookup"><span data-stu-id="fceec-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="fceec-154">すべての製品カテゴリの"Toys"に等しいを返します。</span><span class="sxs-lookup"><span data-stu-id="fceec-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="fceec-155">`http://localhost/Products?$filter=Category`eq 'Toys'</span><span class="sxs-lookup"><span data-stu-id="fceec-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="fceec-156">すべての製品の価格が 10 よりも小さいかを返します。</span><span class="sxs-lookup"><span data-stu-id="fceec-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="fceec-157">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="fceec-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="fceec-158">論理演算子: すべての製品を返す価格、> = 5 と価格 < 15 を = です。</span><span class="sxs-lookup"><span data-stu-id="fceec-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="fceec-159">`http://localhost/Products?$filter=Price`ge 5 と価格 le 15</span><span class="sxs-lookup"><span data-stu-id="fceec-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="fceec-160">文字列関数: 名に"zz"のすべての製品を返します。</span><span class="sxs-lookup"><span data-stu-id="fceec-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="fceec-161">日付関数: 2005年より後 ReleaseDate のすべての製品を返します。</span><span class="sxs-lookup"><span data-stu-id="fceec-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="fceec-162">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="fceec-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="fceec-163">**並べ替え**</span><span class="sxs-lookup"><span data-stu-id="fceec-163">**Sorting**</span></span>

<span data-ttu-id="fceec-164">結果を並べ替えるには、$orderby フィルターを使用します。</span><span class="sxs-lookup"><span data-stu-id="fceec-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="fceec-165">価格で並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="fceec-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="fceec-166">価格 (最高ランク) の順序を降順で並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="fceec-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="fceec-167">カテゴリ別に並べ替えるしてから、価格のカテゴリで降順に並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="fceec-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="fceec-168">サーバー ドリブン ページング</span><span class="sxs-lookup"><span data-stu-id="fceec-168">Server-Driven Paging</span></span>

<span data-ttu-id="fceec-169">送信しない場合は、データベースには、数百万レコードにはが含まれています、すべて 1 つのペイロードにします。</span><span class="sxs-lookup"><span data-stu-id="fceec-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="fceec-170">これを防ぐためには、サーバーは、1 つの応答で送信されるエントリの数を制限できます。</span><span class="sxs-lookup"><span data-stu-id="fceec-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="fceec-171">サーバー ページングを有効にするには設定、 **PageSize**プロパティに、 **Queryable**属性。</span><span class="sxs-lookup"><span data-stu-id="fceec-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="fceec-172">値は、返される項目の最大数です。</span><span class="sxs-lookup"><span data-stu-id="fceec-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="fceec-173">コント ローラーには、OData 形式が返された場合、応答本文には次のデータ ページへのリンクが含まれます。</span><span class="sxs-lookup"><span data-stu-id="fceec-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="fceec-174">クライアントは、次のページをフェッチするのにこのリンクを使用できます。</span><span class="sxs-lookup"><span data-stu-id="fceec-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="fceec-175">結果セット内のエントリの合計数については、クライアント設定できます、値を持つ $inlinecount クエリ オプション。"allpages"。</span><span class="sxs-lookup"><span data-stu-id="fceec-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="fceec-176">値は、"allpages"は、応答の合計数を追加するには、サーバーを示します。</span><span class="sxs-lookup"><span data-stu-id="fceec-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="fceec-177">次のページへのリンクとインライン カウント OData 形式が必要です。</span><span class="sxs-lookup"><span data-stu-id="fceec-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="fceec-178">その理由は、OData が、リンクとカウントを保持するために、応答本文で特別なフィールドを定義します。</span><span class="sxs-lookup"><span data-stu-id="fceec-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="fceec-179">非 OData 形式は、することでクエリ結果をラップすることによって、次のページのリンクとインライン カウントをサポートすること、 **PageResult&lt;T&gt;** オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="fceec-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="fceec-180">ただし、もう少しコードが必要です。</span><span class="sxs-lookup"><span data-stu-id="fceec-180">However, it requires a bit more code.</span></span> <span data-ttu-id="fceec-181">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="fceec-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="fceec-182">JSON 応答の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="fceec-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="fceec-183">クエリ オプションを制限します。</span><span class="sxs-lookup"><span data-stu-id="fceec-183">Limiting the Query Options</span></span>

<span data-ttu-id="fceec-184">クエリ オプションは、クライアントに多数のサーバーで実行されるクエリに制御を提供します。</span><span class="sxs-lookup"><span data-stu-id="fceec-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="fceec-185">場合によっては、セキュリティやパフォーマンス上の理由から使用可能なオプションを制限することができます。</span><span class="sxs-lookup"><span data-stu-id="fceec-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="fceec-186">**[Queryable]**属性がいくつかビルド プロパティでこのです。</span><span class="sxs-lookup"><span data-stu-id="fceec-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="fceec-187">以下に例をいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="fceec-187">Here are some examples.</span></span>

<span data-ttu-id="fceec-188">ページングとそれ以外のものをサポートするために $skip と $top、のみを許可します。</span><span class="sxs-lookup"><span data-stu-id="fceec-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="fceec-189">データベースでインデックス化されていないプロパティでの並べ替えを防ぐために、特定のプロパティだけが順序付けを許可します。</span><span class="sxs-lookup"><span data-stu-id="fceec-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="fceec-190">"Eq"論理関数ですがないその他の論理関数を許可します。</span><span class="sxs-lookup"><span data-stu-id="fceec-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="fceec-191">すべての算術演算子をことはできません。</span><span class="sxs-lookup"><span data-stu-id="fceec-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="fceec-192">作成することによりグローバルにオプションを制限することができます、 **QueryableAttribute**インスタンスに渡すと、 **EnableQuerySupport**関数。</span><span class="sxs-lookup"><span data-stu-id="fceec-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="fceec-193">クエリ オプションの直接呼び出し</span><span class="sxs-lookup"><span data-stu-id="fceec-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="fceec-194">使用する代わりに、 **[Queryable]**属性に、コント ローラーで直接クエリ オプションを呼び出すことができます。</span><span class="sxs-lookup"><span data-stu-id="fceec-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="fceec-195">これを行うには、追加、 **ODataQueryOptions**コント ローラー メソッドへのパラメーターです。</span><span class="sxs-lookup"><span data-stu-id="fceec-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="fceec-196">この場合、する必要はありません、 **[Queryable]**属性。</span><span class="sxs-lookup"><span data-stu-id="fceec-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="fceec-197">Web API には追加、 **ODataQueryOptions** URI からクエリ文字列。</span><span class="sxs-lookup"><span data-stu-id="fceec-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="fceec-198">適用するには、クエリを渡す、 **IQueryable**を**ApplyTo**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fceec-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="fceec-199">メソッドを返します別**IQueryable**です。</span><span class="sxs-lookup"><span data-stu-id="fceec-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="fceec-200">高度なシナリオではない場合、 **IQueryable**クエリ プロバイダーを調べることができます、 **ODataQueryOptions**し、別のフォームへのクエリ オプションを変換します。</span><span class="sxs-lookup"><span data-stu-id="fceec-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="fceec-201">(たとえば、RaghuRam Nadiminti のブログの投稿を参照してください[翻訳する OData クエリの HQL へ](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx)も含まれていますが、[サンプル](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt))。</span><span class="sxs-lookup"><span data-stu-id="fceec-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="fceec-202">クエリの検証</span><span class="sxs-lookup"><span data-stu-id="fceec-202">Query Validation</span></span>

<span data-ttu-id="fceec-203">**[Queryable]**属性は、実行する前に、クエリを検証します。</span><span class="sxs-lookup"><span data-stu-id="fceec-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="fceec-204">検証のステップは、実行、 **QueryableAttribute.ValidateQuery**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fceec-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="fceec-205">検証プロセスをカスタマイズすることもできます。</span><span class="sxs-lookup"><span data-stu-id="fceec-205">You can also customize the validation process.</span></span>

<span data-ttu-id="fceec-206">参照してください[OData セキュリティ ガイダンス](odata-security-guidance.md)です。</span><span class="sxs-lookup"><span data-stu-id="fceec-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="fceec-207">最初に、オーバーライドされている検証コントロールのいずれかのクラスが定義されている、 **Web.Http.OData.Query.Validators**名前空間。</span><span class="sxs-lookup"><span data-stu-id="fceec-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="fceec-208">たとえば、次の検証コントロール クラスには、$orderby オプションの 'desc' オプションが無効にします。</span><span class="sxs-lookup"><span data-stu-id="fceec-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="fceec-209">サブクラス、 **[Queryable]**をオーバーライドする属性、 **ValidateQuery**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="fceec-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="fceec-210">カスタム属性を設定するか、グローバルに、コント ローラーごと。</span><span class="sxs-lookup"><span data-stu-id="fceec-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="fceec-211">使用している場合**ODataQueryOptions**直接、オプションで、検証コントロールを設定します。</span><span class="sxs-lookup"><span data-stu-id="fceec-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]