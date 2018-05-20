---
title: ASP.NET Core の Razor SDK
author: Rick-Anderson
description: ASP.NET Core の Razor ページを使用して、ページのコーディングに重点を置いたシナリオをより簡略化して、MVC を使用する場合よりも生産性を高める方法について説明します。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: 2cbebb12ccd1098e1950aa7eeb22fab4ffc689e6
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="0d52d-103">ASP.NET Core の Razor SDK</span><span class="sxs-lookup"><span data-stu-id="0d52d-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="0d52d-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0d52d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="0d52d-105">[!INCLUDE[](~/includes/2.1-SDK.md)] には、`Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK) が含まれています。</span><span class="sxs-lookup"><span data-stu-id="0d52d-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="0d52d-106">Razor SDK とは、次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0d52d-106">The Razor SDK:</span></span>

* <span data-ttu-id="0d52d-107">ASP.NET Core MVC ベース プロジェクト用の [Razor](xref:mvc/views/razor) ファイルを含むプロジェクトのビルド、パッケージ、および発行に関わる表示と操作を標準化できます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="0d52d-108">Razor ファイルのコンパイルのカスタマイズを可能にする事前定義されたターゲット、プロパティ、項目のセットを含みます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d52d-109">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="0d52d-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="0d52d-110">Razor SDK を使用する</span><span class="sxs-lookup"><span data-stu-id="0d52d-110">Using the Razor SDK</span></span>

<span data-ttu-id="0d52d-111">多くの Web アプリは、Razor SDK を明示的に参照する必要はありません。</span><span class="sxs-lookup"><span data-stu-id="0d52d-111">Most web apps don't need to expressly reference the Razor SDK.</span></span> 

<span data-ttu-id="0d52d-112">Razor SDK を使用して Razor ビューまたは Razor ページを含むクラス ライブラリを構築する方法は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="0d52d-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="0d52d-113">`Microsoft.NET.Sdk.Razor` の代わりに `Microsoft.NET.Sdk` を使用します。</span><span class="sxs-lookup"><span data-stu-id="0d52d-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* <span data-ttu-id="0d52d-114">Razor ページや Razor ビューをビルドおよびコンパイルするために必要な追加の依存関係を組み込むには、通常 `Microsoft.AspNetCore.Mvc` へのパッケージ参照が必要です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-114">Typically a package reference to `Microsoft.AspNetCore.Mvc` is required to bring in additional dependencies required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="0d52d-115">少なくとも、プロジェクトで次へのパッケージ参照を追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="0d52d-115">At minimum, your project needs to add package references to:</span></span>

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 <span data-ttu-id="0d52d-116">前述のパッケージは、`Microsoft.AspNetCore.Mvc` に組み込まれています。</span><span class="sxs-lookup"><span data-stu-id="0d52d-116">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="0d52d-117">次のマークアップは、Razor SDK で、ASP.NET Core Razor ページ アプリ用の Razor ファイルがビルドされた基本の *.csproj* ファイルです。</span><span class="sxs-lookup"><span data-stu-id="0d52d-117">The following markup shows a basic *.csproj* file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a><span data-ttu-id="0d52d-118">プロパティ</span><span class="sxs-lookup"><span data-stu-id="0d52d-118">Properties</span></span>

<span data-ttu-id="0d52d-119">Razor の SDK の動作は、プロジェクトをビルドする際に次のプロパティにより制御されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-119">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="0d52d-120">`RazorCompileOnBuild` : `true` の場合、プロジェクトをビルドする際に、Razor アセンブリがコンパイルおよび生成されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-120">`RazorCompileOnBuild` : When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="0d52d-121">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-121">Defaults to `true`.</span></span>
* <span data-ttu-id="0d52d-122">`RazorCompileOnPublish` : `true` の場合、プロジェクトを発行する際に、Razor アセンブリがコンパイルおよび生成されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-122">`RazorCompileOnPublish` : When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="0d52d-123">既定値は `true` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-123">Defaults to `true`.</span></span>

<span data-ttu-id="0d52d-124">Razor SDK への入力および出力は、次のプロパティおよび項目を使用して構成されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-124">The following properties and items are used to configure inputs and output to the Razor SDK:</span></span>

| <span data-ttu-id="0d52d-125">項目</span><span class="sxs-lookup"><span data-stu-id="0d52d-125">Items</span></span>                                         | <span data-ttu-id="0d52d-126">説明</span><span class="sxs-lookup"><span data-stu-id="0d52d-126">Description</span></span>                                                                   |
| ------------                                  | -------------                                                                 |
| <span data-ttu-id="0d52d-127">RazorGenerate</span><span class="sxs-lookup"><span data-stu-id="0d52d-127">RazorGenerate</span></span>                                 | <span data-ttu-id="0d52d-128">コードの生成対象に入力する項目要素 (*.cshtml* ファイル) です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-128">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| <span data-ttu-id="0d52d-129">RazorCompile</span><span class="sxs-lookup"><span data-stu-id="0d52d-129">RazorCompile</span></span>                                  | <span data-ttu-id="0d52d-130">Razor コンパイル対象への入力である項目要素 (.cs ファイル) です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-130">Item elements (.cs files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="0d52d-131">Razor アセンブリに追加でコンパイルするファイルを指定するには、この ItemGroup を使用します。</span><span class="sxs-lookup"><span data-stu-id="0d52d-131">Use this ItemGroup to specify additional files to be compiled in to the Razor assembly.</span></span> |
| <span data-ttu-id="0d52d-132">RazorAssemblyAttribute</span><span class="sxs-lookup"><span data-stu-id="0d52d-132">RazorAssemblyAttribute</span></span>                        | <span data-ttu-id="0d52d-133">Razor アセンブリ用の属性をコード生成するために使用する項目要素です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-133">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="0d52d-134">例:</span><span class="sxs-lookup"><span data-stu-id="0d52d-134">For example:</span></span>  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| <span data-ttu-id="0d52d-135">RazorEmbeddedResource</span><span class="sxs-lookup"><span data-stu-id="0d52d-135">RazorEmbeddedResource</span></span>                         | <span data-ttu-id="0d52d-136">生成された Razor アセンブリに埋め込みのリソースとして追加される項目要素です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-136">Item elements added as embedded resources to the generated Razor assembly</span></span> |

| <span data-ttu-id="0d52d-137">プロパティ</span><span class="sxs-lookup"><span data-stu-id="0d52d-137">Property</span></span>                                      | <span data-ttu-id="0d52d-138">説明</span><span class="sxs-lookup"><span data-stu-id="0d52d-138">Description</span></span>                                                                   |
| ------------                                  | -------------                                                                 |
| <span data-ttu-id="0d52d-139">RazorTargetName</span><span class="sxs-lookup"><span data-stu-id="0d52d-139">RazorTargetName</span></span>                               | <span data-ttu-id="0d52d-140">Razor によって生成されたアセンブリの (拡張子なしの) ファイル名です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| <span data-ttu-id="0d52d-141">RazorOutputPath</span><span class="sxs-lookup"><span data-stu-id="0d52d-141">RazorOutputPath</span></span>                               | <span data-ttu-id="0d52d-142">Razor の出力ディレクトリです。</span><span class="sxs-lookup"><span data-stu-id="0d52d-142">The Razor output directory.</span></span>                                      |
| <span data-ttu-id="0d52d-143">RazorCompileToolset</span><span class="sxs-lookup"><span data-stu-id="0d52d-143">RazorCompileToolset</span></span>                           | <span data-ttu-id="0d52d-144">Razor アセンブリをビルドするために使用するツールセットを決定するために使用します。</span><span class="sxs-lookup"><span data-stu-id="0d52d-144">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="0d52d-145">有効な値は、`Implicit` および `PrecompilationTool` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-145">Valid values are `Implicit`, , and `PrecompilationTool`.</span></span> |
| <span data-ttu-id="0d52d-146">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="0d52d-146">EnableDefaultContentItems</span></span>                     | <span data-ttu-id="0d52d-147">`true` の場合、*.cshtml* ファイルなどの特定の種類のファイルがプロジェクトのコンテンツとして含まれます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-147">When `true`, includes certain file types, such as *.cshtml* files, as content in the project.</span></span> <span data-ttu-id="0d52d-148">Microsoft.NET.Sdk.Web を介して参照する場合、*wwwroot* 下のすべてのファイルおよび構成ファイルも含まれます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-148">When referenced via Microsoft.NET.Sdk.Web, also includes all files under *wwwroot*, and config files.</span></span>         |
| <span data-ttu-id="0d52d-149">EnableDefaultRazorGenerateItems</span><span class="sxs-lookup"><span data-stu-id="0d52d-149">EnableDefaultRazorGenerateItems</span></span>               | <span data-ttu-id="0d52d-150">`true` の場合、`RazorGenerate` 項目の `Content` 項目の *.cshtml* ファイルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-150">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| <span data-ttu-id="0d52d-151">GenerateRazorTargetAssemblyInfo</span><span class="sxs-lookup"><span data-stu-id="0d52d-151">GenerateRazorTargetAssemblyInfo</span></span>               | <span data-ttu-id="0d52d-152">`true` の場合、`RazorAssemblyAttribute` で指定された属性を含む *.cs* ファイルが生成され、それがコンパイルの出力が含まれます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-152">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes it in the compile output.</span></span> |
| <span data-ttu-id="0d52d-153">EnableDefaultRazorTargetAssemblyInfoAttributes</span><span class="sxs-lookup"><span data-stu-id="0d52d-153">EnableDefaultRazorTargetAssemblyInfoAttributes</span></span> | <span data-ttu-id="0d52d-154">`true` の場合、`RazorAssemblyAttribute` にアセンブリ属性の既定のセットが追加されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-154">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| <span data-ttu-id="0d52d-155">CopyRazorGenerateFilesToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="0d52d-155">CopyRazorGenerateFilesToPublishDirectory</span></span>       | <span data-ttu-id="0d52d-156">`true` の場合、RazorGenerate 項目 (*.cshtml* ファイル) が発行ディレクトリにコピーされます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-156">When `true`, copies RazorGenerate items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="0d52d-157">ビルド時または発行時にコンパイルに含まれる場合、通常発行済みアプリケーションには Razor ファイルは不要です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-157">Typically Razor files are not needed for a published application if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="0d52d-158">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-158">Defaults to `false`.</span></span> |
| <span data-ttu-id="0d52d-159">CopyRefAssembliesToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="0d52d-159">CopyRefAssembliesToPublishDirectory</span></span>            | <span data-ttu-id="0d52d-160">`true` の場合、発行ディレクトリに参照アセンブリ項目がコピーされます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-160">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="0d52d-161">通常、Razor のコンパイルがビルド時または発行時に発生する場合、発行済みアプリケーションには参照アセンブリは不要です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-161">Typically reference assemblies are not needed for a published application if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="0d52d-162">これが `true` に設定されている場合、発行済みのアプリケーションで実行時のコンパイルが必要な場合、実行時に cshtml ファイルが変更されたり、埋め込みのビューが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-162">Set to `true`, if your published application requires runtime compilation, for example, modifies cshtml files at runtime, or uses embedded views.</span></span> <span data-ttu-id="0d52d-163">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-163">Defaults to `false`.</span></span> |
| <span data-ttu-id="0d52d-164">IncludeRazorContentInPack</span><span class="sxs-lookup"><span data-stu-id="0d52d-164">IncludeRazorContentInPack</span></span>                      | <span data-ttu-id="0d52d-165">`true` の場合、すべての Razor コンテンツ アイテム (*.cshtml* ファイル) は、生成された NuGet パッケージに含まれるものとしてマークされます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-165">When `true`, all Razor content items (*.cshtml* files) will be marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="0d52d-166">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-166">Defaults to `false`.</span></span> |
| <span data-ttu-id="0d52d-167">EmbedRazorGenerateSources</span><span class="sxs-lookup"><span data-stu-id="0d52d-167">EmbedRazorGenerateSources</span></span> | <span data-ttu-id="0d52d-168">`true` の場合、生成された Razor アセンブリに、埋め込みファイルとして RazorGenerate (*.cshtml*) 項目が追加されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-168">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="0d52d-169">既定値は `false` です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-169">Defaults to `false`.</span></span> |
| <span data-ttu-id="0d52d-170">UseRazorBuildServer</span><span class="sxs-lookup"><span data-stu-id="0d52d-170">UseRazorBuildServer</span></span>                           | <span data-ttu-id="0d52d-171">`true` の場合、コードの生成作業をオフロードするために、永続的なビルド サーバーが使用されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-171">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="0d52d-172">既定値は、`UseSharedCompilation` の値です。</span><span class="sxs-lookup"><span data-stu-id="0d52d-172">Defaults to the value of `UseSharedCompilation`.</span></span> |

### <a name="targets"></a><span data-ttu-id="0d52d-173">ターゲット</span><span class="sxs-lookup"><span data-stu-id="0d52d-173">Targets</span></span>
<span data-ttu-id="0d52d-174">Razor SDK では、次の 2 つの主要なターゲットが定義されています。</span><span class="sxs-lookup"><span data-stu-id="0d52d-174">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="0d52d-175">`RazorGenerate`: コードにより RazorGenerate 項目要素から *.cs* ファイルが生成されます。</span><span class="sxs-lookup"><span data-stu-id="0d52d-175">`RazorGenerate` - Code generates *.cs* files from RazorGenerate item elements.</span></span> <span data-ttu-id="0d52d-176">このターゲットの前後に実行する追加のターゲットを指定するには、`RazorGenerateDependsOn` プロパティを使用します。</span><span class="sxs-lookup"><span data-stu-id="0d52d-176">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="0d52d-177">`RazorCompile`: Razor アセンブリに生成された *.cs* ファイルをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="0d52d-177">`RazorCompile` - Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="0d52d-178">このターゲットの前後に実行する追加のターゲットを指定するには、`RazorCompileDependsOn` を使用します。</span><span class="sxs-lookup"><span data-stu-id="0d52d-178">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>