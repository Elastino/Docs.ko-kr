---
title: "부분 뷰"
author: ardalis
description: "ASP.NET Core mvc에서 부분 뷰를 사용 하 여"
keywords: "ASP.NET Core, 부분 뷰, 부분"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 4be1b12c-b74e-44ff-826b-99ce86e8d464
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/partial
ms.openlocfilehash: 60f5255ca31accbffffec18053b29810977a5ff1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
# <a name="partial-views"></a><span data-ttu-id="fe540-104">부분 뷰</span><span class="sxs-lookup"><span data-stu-id="fe540-104">Partial Views</span></span>

<span data-ttu-id="fe540-105">여 [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe540-105">By [Steve Smith](https://ardalis.com/), [Maher JENDOUBI](https://twitter.com/maherjend), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fe540-106">ASP.NET Core MVC는 재사용 가능한 파트의 서로 다른 뷰 간에 공유 하려는 웹 페이지에 있는 경우에 유용 부분 뷰를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-106">ASP.NET Core MVC supports partial views, which are useful when you have reusable parts of web pages you want to share between different views.</span></span>

<span data-ttu-id="fe540-107">[샘플 코드 보기 또는 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fe540-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-are-partial-views"></a><span data-ttu-id="fe540-108">부분 뷰는 무엇입니까?</span><span class="sxs-lookup"><span data-stu-id="fe540-108">What are Partial Views?</span></span>

<span data-ttu-id="fe540-109">부분 뷰는 다른 뷰 내에서 렌더링 되는 뷰입니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-109">A partial view is a view that is rendered within another view.</span></span> <span data-ttu-id="fe540-110">부분 뷰를 실행 하 여 생성 된 HTML 출력을 호출 (또는 부모) 보기에 렌더링 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-110">The HTML output generated by executing the partial view is rendered into the calling (or parent) view.</span></span> <span data-ttu-id="fe540-111">부분 뷰 같이 뷰를 사용 하 여는 *.cshtml* 파일 확장명입니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-111">Like views, partial views use the *.cshtml* file extension.</span></span>

## <a name="when-should-i-use-partial-views"></a><span data-ttu-id="fe540-112">부분 뷰는 경우 사용 해야 합니까?</span><span class="sxs-lookup"><span data-stu-id="fe540-112">When Should I Use Partial Views?</span></span>

<span data-ttu-id="fe540-113">부분 뷰는 효과적으로 큰 뷰를 더 작은 구성 요소로 분리 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-113">Partial views are an effective way of breaking up large views into smaller components.</span></span> <span data-ttu-id="fe540-114">콘텐츠 보기의 중복을 줄일 수 있으며 뷰 요소를 다시 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-114">They can reduce duplication of view content and allow view elements to be reused.</span></span> <span data-ttu-id="fe540-115">일반 레이아웃 요소에 지정 해야 [_Layout.cshtml](layout.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-115">Common layout elements should be specified in [_Layout.cshtml](layout.md).</span></span> <span data-ttu-id="fe540-116">비 레이아웃 다시 사용할 수 있는 콘텐츠 부분 뷰 개체로 캡슐화 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-116">Non-layout reusable content can be encapsulated into partial views.</span></span>

<span data-ttu-id="fe540-117">논리 조각으로 구성 하는 복잡 한 페이지를 사용 하도록 설정한 경우에 자체 부분 뷰로 각 부분에 맞게 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-117">If you have a complex page made up of several logical pieces, it can be helpful to work with each piece as its own partial view.</span></span> <span data-ttu-id="fe540-118">페이지의 나머지 부분과 분리에서 페이지의 각 부분을 볼 수 있습니다만 포함 되어 있으므로 전체 페이지 구조와를 부분 뷰를 렌더링 하기 페이지 자체에 대 한 보기 매핑되며 훨씬 더 간단 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-118">Each piece of the page can be viewed in isolation from the rest of the page, and the view for the page itself becomes much simpler since it only contains the overall page structure and calls to render the partial views.</span></span>

<span data-ttu-id="fe540-119">팁: 따라는 [하지 반복 직접 원칙](http://deviq.com/don-t-repeat-yourself/) 보기에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-119">Tip: Follow the [Don't Repeat Yourself Principle](http://deviq.com/don-t-repeat-yourself/) in your views.</span></span>

## <a name="declaring-partial-views"></a><span data-ttu-id="fe540-120">부분 뷰를 선언합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-120">Declaring Partial Views</span></span>

<span data-ttu-id="fe540-121">부분 뷰를 다른 뷰와 마찬가지로 만들어집니다: 만들면는 *.cshtml* 내에 파일이 *뷰* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-121">Partial views are created like any other view: you create a *.cshtml* file within the *Views* folder.</span></span> <span data-ttu-id="fe540-122">부분 뷰 및 일반 보기 간의 의미 체계 차이점이-다르게 렌더링만 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-122">There is no semantic difference between a partial view and a regular view - they are just rendered differently.</span></span> <span data-ttu-id="fe540-123">컨트롤러의 직접에서 반환 되는 뷰를 사용할 수 있습니다 `ViewResult`, 부분 뷰로 같은 보기를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-123">You can have a view that is returned directly from a controller's `ViewResult`, and the same view can be used as a partial view.</span></span> <span data-ttu-id="fe540-124">부분 뷰 실행 하지 않는 뷰와 부분 뷰 렌더링 되는 방식 간의 주요 차이점은 *_viewstart.vbhtml* (뷰 수행-자세한 정보에 대 한 동안 *_viewstart.vbhtml* 에 [레이아웃 ](layout.md)).</span><span class="sxs-lookup"><span data-stu-id="fe540-124">The main difference between how a view and a partial view are rendered is that partial views do not run *_ViewStart.cshtml* (while views do - learn more about *_ViewStart.cshtml* in [Layout](layout.md)).</span></span>

## <a name="referencing-a-partial-view"></a><span data-ttu-id="fe540-125">부분 뷰를 참조합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-125">Referencing a Partial View</span></span>

<span data-ttu-id="fe540-126">페이지 보기 내에서 여러 가지가 부분 뷰를 렌더링할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-126">From within a view page, there are several ways in which you can render a partial view.</span></span> <span data-ttu-id="fe540-127">사용 하는 가장 간단한 것 `Html.Partial`를 반환 하는 `IHtmlString` 접두사로 사용 하 여 호출 하 여 참조 될 수 있습니다 및 `@`:</span><span class="sxs-lookup"><span data-stu-id="fe540-127">The simplest is to use `Html.Partial`, which returns an `IHtmlString` and can be referenced by prefixing the call with `@`:</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=9)]

<span data-ttu-id="fe540-128">`PartialAsync` 메서드는 부분 (보기에는 코드는 일반적으로 권장 되지 않습니다)는 없지만 비동기 코드가 포함 된 뷰를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-128">The `PartialAsync` method is available for partial views containing asynchronous code (although code in views is generally discouraged):</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=8)]

<span data-ttu-id="fe540-129">부분 뷰를 렌더링할 수도 있고 `RenderPartial`합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-129">You can render a partial view with `RenderPartial`.</span></span> <span data-ttu-id="fe540-130">이 메서드는 결과 반환 하지 않습니다. 렌더링된 된 출력을 응답에 직접를 스트리밍합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-130">This method doesn't return a result; it streams the rendered output directly to the response.</span></span> <span data-ttu-id="fe540-131">Razor 코드 블록 내에서 호출할 수 해야 결과 반환 하지 않습니다 (호출할 수도 있습니다 `RenderPartialAsync` 필요한 경우):</span><span class="sxs-lookup"><span data-stu-id="fe540-131">Because it doesn't return a result, it must be called within a Razor code block (you can also call `RenderPartialAsync` if necessary):</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Home/About.cshtml?range=10-12)]

<span data-ttu-id="fe540-132">결과 직접 스트리밍합니다 때문에 `RenderPartial` 및 `RenderPartialAsync` 일부 시나리오에서 더 잘 수행 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-132">Because it streams the result directly, `RenderPartial` and `RenderPartialAsync` may perform better in some scenarios.</span></span> <span data-ttu-id="fe540-133">그러나 사용 하는 것이 좋습니다 대부분의 경우 `Partial` 및 `PartialAsync`합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-133">However, in most cases it's recommended you use `Partial` and `PartialAsync`.</span></span>

> [!NOTE]
> <span data-ttu-id="fe540-134">보기를 코드를 실행 해야 할 경우 권장된 패턴을 사용 하는 [뷰 구성 요소](view-components.md) 부분 뷰를 대신 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-134">If your views need to execute code, the recommended pattern is to use a [view component](view-components.md) instead of a partial view.</span></span>

### <a name="partial-view-discovery"></a><span data-ttu-id="fe540-135">부분 뷰 검색</span><span class="sxs-lookup"><span data-stu-id="fe540-135">Partial View Discovery</span></span>

<span data-ttu-id="fe540-136">부분 뷰를 참조 하는 경우 여러 가지 방법으로 해당 위치를 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-136">When referencing a partial view, you can refer to its location in several ways:</span></span>

```text
// Uses a view in current folder with this name
// If none is found, searches the Shared folder
@Html.Partial("ViewName")

// A view with this name must be in the same folder
@Html.Partial("ViewName.cshtml")

// Locate the view based on the application root
// Paths that start with "/" or "~/" refer to the application root
@Html.Partial("~/Views/Folder/ViewName.cshtml")
@Html.Partial("/Views/Folder/ViewName.cshtml")

// Locate the view using relative paths
@Html.Partial("../Account/LoginPartial.cshtml")
```

<span data-ttu-id="fe540-137">서로 다른 뷰 폴더에 이름이 같은 다른 부분 뷰를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-137">You can have different partial views with the same name in different view folders.</span></span> <span data-ttu-id="fe540-138">확장명을 제외한 파일 이름으로 뷰를 참조할 때 각 폴더의 보기와 같은 폴더에 부분 뷰를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-138">When referencing the views by name (without file extension), views in each folder will use the partial view in the same folder with them.</span></span> <span data-ttu-id="fe540-139">또한에 넣어서 기본 부분 뷰를 사용 하려면 지정할 수 있습니다는 *Shared* 폴더입니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-139">You can also specify a default partial view to use, placing it in the *Shared* folder.</span></span> <span data-ttu-id="fe540-140">공유 부분 뷰의 부분 뷰의의 고유 버전을 갖지 않는 모든 보기에서 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-140">The shared partial view will be used by any views that don't have their own version of the partial view.</span></span> <span data-ttu-id="fe540-141">기본 부분 뷰를 사용할 수 있습니다 (에서 *Shared*)는 부모 보기와 같은 폴더에 같은 이름의 부분 뷰에 의해 재정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-141">You can have a default partial view (in *Shared*), which is overridden by a partial view with the same name in the same folder as the parent view.</span></span>

<span data-ttu-id="fe540-142">부분 뷰 수 *연결*합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-142">Partial views can be *chained*.</span></span> <span data-ttu-id="fe540-143">즉, 부분 뷰 (으로 루프를 만들지 않으면) 다른 부분 뷰를 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-143">That is, a partial view can call another partial view (as long as you don't create a loop).</span></span> <span data-ttu-id="fe540-144">각 뷰나 부분 뷰 내에서 상대 경로 항상 해당 보기, 하지 루트 또는 부모 보기를 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-144">Within each view or partial view, relative paths are always relative to that view, not the root or parent view.</span></span>

> [!NOTE]
> <span data-ttu-id="fe540-145">선언 하는 경우는 [Razor](razor.md) `section` 부분 뷰에서 않을 해당 부모에 게 표시; 부분 뷰로 제한 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-145">If you declare a [Razor](razor.md) `section` in a partial view, it will not be visible to its parent(s); it will be limited to the partial view.</span></span>

## <a name="accessing-data-from-partial-views"></a><span data-ttu-id="fe540-146">부분 뷰에서 데이터에 액세스</span><span class="sxs-lookup"><span data-stu-id="fe540-146">Accessing Data From Partial Views</span></span>

<span data-ttu-id="fe540-147">부모 뷰의 복사본을 가져와서 부분 뷰가 인스턴스화되면 `ViewData` 사전입니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-147">When a partial view is instantiated, it gets a copy of the parent view's `ViewData` dictionary.</span></span> <span data-ttu-id="fe540-148">부분 뷰 내에서 데이터에 대 한 업데이트는 부모 뷰에 유지 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-148">Updates made to the data within the partial view are not persisted to the parent view.</span></span> <span data-ttu-id="fe540-149">`ViewData`부분에서 변경 된 부분 뷰는 반환 될 때 보기 손실 됩니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-149">`ViewData` changed in a partial view is lost when the partial view returns.</span></span>

<span data-ttu-id="fe540-150">인스턴스를 전달할 수 `ViewDataDictionary` 부분 뷰로:</span><span class="sxs-lookup"><span data-stu-id="fe540-150">You can pass an instance of `ViewDataDictionary` to the partial view:</span></span>

```csharp
@Html.Partial("PartialName", customViewData)
   ```

<span data-ttu-id="fe540-151">부분 뷰를 모델을 전달할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-151">You can also pass a model into a partial view.</span></span> <span data-ttu-id="fe540-152">이 페이지의 보기 모델 또는 일부 일부를 사용자 지정 개체 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-152">This can be the page's view model, or some portion of it, or a custom object.</span></span> <span data-ttu-id="fe540-153">모델을 전달할 수 있습니다 `Partial`,`PartialAsync`, `RenderPartial`, 또는 `RenderPartialAsync`:</span><span class="sxs-lookup"><span data-stu-id="fe540-153">You can pass a model to `Partial`,`PartialAsync`, `RenderPartial`, or `RenderPartialAsync`:</span></span>

```csharp
@Html.Partial("PartialName", viewModel)
   ```

<span data-ttu-id="fe540-154">인스턴스를 전달할 수 `ViewDataDictionary` 및 부분 뷰를 뷰 모델:</span><span class="sxs-lookup"><span data-stu-id="fe540-154">You can pass an instance of `ViewDataDictionary` and a view model to a partial view:</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml?range=15-16)]

<span data-ttu-id="fe540-155">다음 태그는 *Views/Articles/Read.cshtml* 두 부분 뷰를 포함 하는 뷰.</span><span class="sxs-lookup"><span data-stu-id="fe540-155">The markup below shows the *Views/Articles/Read.cshtml* view which contains two partial views.</span></span> <span data-ttu-id="fe540-156">두 번째 부분 뷰는 모델에 전달 하 고 `ViewData` 부분 뷰를 합니다.</span><span class="sxs-lookup"><span data-stu-id="fe540-156">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="fe540-157">새로운 전달할 수 `ViewData` 기존 유지 하면서 사전 `ViewData` 생성자 오버 로드를 사용 하는 경우는 `ViewDataDictionary` 아래의 강조 표시 된:</span><span class="sxs-lookup"><span data-stu-id="fe540-157">You can pass new `ViewData` dictionary while retaining the existing `ViewData` if you use the constructor overload of the `ViewDataDictionary` highlighted below:</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/Read.cshtml)]

<span data-ttu-id="fe540-158">*뷰/공유/AuthorPartial*:</span><span class="sxs-lookup"><span data-stu-id="fe540-158">*Views/Shared/AuthorPartial*:</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Shared/AuthorPartial.cshtml)]

<span data-ttu-id="fe540-159">*ArticleSection* 부분:</span><span class="sxs-lookup"><span data-stu-id="fe540-159">The *ArticleSection* partial:</span></span>

[!code-html[Main](partial/sample/src/PartialViewsSample/Views/Articles/ArticleSection.cshtml)]

<span data-ttu-id="fe540-160">런타임에 부분 렌더링 되는 부모 보기로 렌더링 되는 자체 공유 내 *_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="fe540-160">At runtime, the partials are rendered into the parent view, which itself is rendered within the shared *_Layout.cshtml*</span></span>

![부분 뷰 출력](partial/_static/output.png)