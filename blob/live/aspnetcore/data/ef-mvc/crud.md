---
title: "ASP.NET Core MVC EF 코어 CRUD-2 / 10 인"
author: tdykstra
description: 
keywords: "ASP.NET Core, Entity Framework Core, CRUD, 만들기, 읽기, 업데이트, 삭제"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: 9fc2b4c126c4d109deb2125f0db70a355c04eb15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/10/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a><span data-ttu-id="102ac-103">만들기, 읽기, 업데이트 및 삭제-EF 코어 ASP.NET Core MVC 자습서 (2 / 10)</span><span class="sxs-lookup"><span data-stu-id="102ac-103">Create, Read, Update, and Delete - EF Core with ASP.NET Core MVC tutorial (2 of 10)</span></span>

<span data-ttu-id="102ac-104">여 [Tom Dykstra](https://github.com/tdykstra) 및 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="102ac-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="102ac-105">Contoso 대학 샘플 웹 응용 프로그램에는 Entity Framework Core 및 Visual Studio를 사용 하 여 ASP.NET Core MVC 웹 응용 프로그램을 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="102ac-106">자습서 시리즈에 대 한 정보를 참조 하십시오. [시리즈의 첫 번째 자습서](intro.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="102ac-107">이전 자습서에서 저장 하 고 Entity Framework 및 SQL Server LocalDB를 사용 하 여 데이터를 표시 하는 MVC 응용 프로그램을 만들었습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-107">In the previous tutorial, you created an MVC application that stores and displays data using the Entity Framework and SQL Server LocalDB.</span></span> <span data-ttu-id="102ac-108">이 자습서를 검토 하는 CRUD 사용자 지정 (만들기, 읽기, 업데이트, 삭제)는 MVC 기반 구조는 자동으로에 만드는 코드를 사용자에 대 한 컨트롤러와 뷰.</span><span class="sxs-lookup"><span data-stu-id="102ac-108">In this tutorial, you'll review and customize the CRUD (create, read, update, delete) code that the MVC scaffolding automatically creates for you in controllers and views.</span></span>

> [!NOTE] 
> <span data-ttu-id="102ac-109">컨트롤러와 데이터 액세스 계층 간에 추상화 계층을 만들기 위해 리포지토리 패턴을 구현 하는 일반적인이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-109">It's a common practice to implement the repository pattern in order to create an abstraction layer between your controller and the data access layer.</span></span> <span data-ttu-id="102ac-110">이 자습서를 간단 하 고 교육 자체 Entity Framework를 사용 하는 방법에 초점을 맞추고 유지 하려면 저장소를 사용 하지 않아 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-110">To keep these tutorials simple and focused on teaching how to use the Entity Framework itself, they don't use repositories.</span></span> <span data-ttu-id="102ac-111">EF의 저장소에 대 한 정보를 참조 하십시오. [이 시리즈의 마지막 자습서](advanced.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-111">For information about repositories with EF, see [the last tutorial in this series](advanced.md).</span></span>

<span data-ttu-id="102ac-112">이 자습서에서는 다음 웹 페이지를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-112">In this tutorial, you'll work with the following web pages:</span></span>

![학생 세부 정보 페이지](crud/_static/student-details.png)

![학생 만들기 페이지](crud/_static/student-create.png)

![학생 편집 페이지](crud/_static/student-edit.png)

![학생 삭제 페이지](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a><span data-ttu-id="102ac-117">사용자 지정 세부 정보 페이지</span><span class="sxs-lookup"><span data-stu-id="102ac-117">Customize the Details page</span></span>

<span data-ttu-id="102ac-118">학생 인덱스 페이지에 대 한 스 캐 폴드 코드에서 제외 된 `Enrollments` 속성을 속성 컬렉션을 보유 하기 때문에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-118">The scaffolded code for the Students Index page left out the `Enrollments` property, because that property holds a collection.</span></span> <span data-ttu-id="102ac-119">에 **세부 정보** 페이지에서 HTML 테이블에 컬렉션의 내용을 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-119">In the **Details** page, you'll display the contents of the collection in an HTML table.</span></span>

<span data-ttu-id="102ac-120">*Controllers/StudentsController.cs*, 세부 정보에 대 한 작업 메서드를 사용 하 여 볼는 `SingleOrDefaultAsync` 메서드는 단일 검색를 `Student` 엔터티.</span><span class="sxs-lookup"><span data-stu-id="102ac-120">In *Controllers/StudentsController.cs*, the action method for the Details view uses the `SingleOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="102ac-121">호출 하는 코드를 추가 `Include`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-121">Add code that calls `Include`.</span></span> <span data-ttu-id="102ac-122">`ThenInclude`및 `AsNoTracking` 메서드를 다음 강조 표시 된 코드에 나와 있는 것 처럼 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-122">`ThenInclude`,  and `AsNoTracking` methods, as shown in the following highlighted code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="102ac-123">`Include` 및 `ThenInclude` 메서드 인해 로드 컨텍스트는 `Student.Enrollments` 탐색 속성 및 각 등록 내는 `Enrollment.Course` 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-123">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span>  <span data-ttu-id="102ac-124">이러한 방법에 대해 자세히 알아봅니다는 [관련된 데이터를 읽는](read-related-data.md) 자습서입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-124">You'll learn more about these methods in the [reading related data](read-related-data.md) tutorial.</span></span>

<span data-ttu-id="102ac-125">`AsNoTracking` 메서드는 반환 되는 엔터티를 현재 컨텍스트 수명에서 업데이트 되지 것입니다 없는 시나리오에서 성능을 향상 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-125">The `AsNoTracking` method improves performance in scenarios where the entities returned will not be updated in the current context's lifetime.</span></span> <span data-ttu-id="102ac-126">에 대 한 자세히 알아보세요 `AsNoTracking` 이 자습서의 끝에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-126">You'll learn more about `AsNoTracking` at the end of this tutorial.</span></span>

### <a name="route-data"></a><span data-ttu-id="102ac-127">경로 데이터</span><span class="sxs-lookup"><span data-stu-id="102ac-127">Route data</span></span>

<span data-ttu-id="102ac-128">에 전달 되는 키 값의 `Details` 메서드에서 가져온 *데이터 전달할*합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-128">The key value that is passed to the `Details` method comes from *route data*.</span></span> <span data-ttu-id="102ac-129">경로 데이터는 모델 바인더는 URL의 세그먼트에 있는 데이터입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-129">Route data is data that the model binder found in a segment of the URL.</span></span> <span data-ttu-id="102ac-130">예를 들어 기본 경로 세그먼트를 컨트롤러, 작업 및 id를 지정합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-130">For example, the default route specifies controller, action, and id segments:</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

<span data-ttu-id="102ac-131">기본 경로 작업으로 인덱스와 1; id로 컨트롤러로 강사를 매핑합니다 다음 url 이들은 경로 데이터 값입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-131">In the following URL, the default route maps Instructor as the controller, Index as the action, and 1 as the id; these are route data values.</span></span>

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

<span data-ttu-id="102ac-132">URL의 마지막 부분 ("? courseID 2021 =") 쿼리 문자열 값입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-132">The last part of the URL ("?courseID=2021") is a query string value.</span></span> <span data-ttu-id="102ac-133">모델 바인더는 ID 값을 전달할 수도 `Details` 메서드 `id` 매개 변수는 쿼리 문자열 값으로 전달 하는 경우:</span><span class="sxs-lookup"><span data-stu-id="102ac-133">The model binder will also pass the ID value to the `Details` method `id` parameter if you pass it as a query string value:</span></span>

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

<span data-ttu-id="102ac-134">인덱스 페이지에 하이퍼링크 Url Razor 보기에서 태그 도우미 문에 의해 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-134">In the Index page, hyperlink URLs are created by tag helper statements in the Razor view.</span></span> <span data-ttu-id="102ac-135">다음 Razor 코드에서의 `id` 매개 변수 이므로 기본 경로 일치 `id` 경로 데이터에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-135">In the following Razor code, the `id` parameter matches the default route, so `id` is added to the route data.</span></span>

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

<span data-ttu-id="102ac-136">다음 HTML 생성 때 `item.ID` 은 6:</span><span class="sxs-lookup"><span data-stu-id="102ac-136">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit/6">Edit</a>
```

<span data-ttu-id="102ac-137">다음 Razor 코드에서 `studentID` 쿼리 문자열로 추가 기본 경로에 매개 변수가 일치 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-137">In the following Razor code, `studentID` doesn't match a parameter in the default route, so it's added as a query string.</span></span>

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

<span data-ttu-id="102ac-138">다음 HTML 생성 때 `item.ID` 은 6:</span><span class="sxs-lookup"><span data-stu-id="102ac-138">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

<span data-ttu-id="102ac-139">태그 도우미에 대 한 자세한 내용은 참조 [태그 도우미에서 ASP.NET Core](xref:mvc/views/tag-helpers/intro)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-139">For more information about tag helpers, see [Tag helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="add-enrollments-to-the-details-view"></a><span data-ttu-id="102ac-140">등록 세부 정보 보기에 추가</span><span class="sxs-lookup"><span data-stu-id="102ac-140">Add enrollments to the Details view</span></span>

<span data-ttu-id="102ac-141">열기 *Views/Students/Details.cshtml*합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-141">Open *Views/Students/Details.cshtml*.</span></span> <span data-ttu-id="102ac-142">사용 하 여 각 필드는 표시 `DisplayNameFor` 및 `DisplayFor` 다음 예제와 같이 도우미 클래스:</span><span class="sxs-lookup"><span data-stu-id="102ac-142">Each field is displayed using `DisplayNameFor` and `DisplayFor` helpers, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

<span data-ttu-id="102ac-143">마지막 필드 뒤와 닫는 직전 `</dl>` 태그 등록의 목록을 표시 하려면 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-143">After the last field and immediately before the closing `</dl>` tag, add the following code to display a list of enrollments:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

<span data-ttu-id="102ac-144">코드 들여쓰기 위치는 코드를 붙여 넣은 후 잘못 된 경우 CTRL-K-D를 수정한 후에 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-144">If code indentation is wrong after you paste the code, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="102ac-145">이 코드의 엔터티는 반복의 `Enrollments` 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-145">This code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="102ac-146">각 등록 과정 제목과 등급 표시합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-146">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="102ac-147">에 저장 된 과정 엔터티에서 과정 이름을 검색 하는 `Course` 등록 엔터티의 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-147">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="102ac-148">응용 프로그램을 실행, 선택는 **학생** 탭을 클릭는 **세부 정보** 학생에 대 한 링크입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-148">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="102ac-149">선택한 학생 과정 및 등급의 목록이 표시:</span><span class="sxs-lookup"><span data-stu-id="102ac-149">You see the list of courses and grades for the selected student:</span></span>

![학생 세부 정보 페이지](crud/_static/student-details.png)

## <a name="update-the-create-page"></a><span data-ttu-id="102ac-151">업데이트 만들기 페이지</span><span class="sxs-lookup"><span data-stu-id="102ac-151">Update the Create page</span></span>

<span data-ttu-id="102ac-152">*StudentsController.cs*, 수정 된 HttpPost `Create` try / catch 블록 추가 및 ID를 제거 하 여 메서드는 `Bind` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-152">In *StudentsController.cs*, modify the HttpPost `Create` method by adding a try-catch block and removing ID from the `Bind` attribute.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

<span data-ttu-id="102ac-153">이 코드는 학생 엔터티를 ASP.NET MVC 모델 바인더를 통해 만든 학생 엔터티 설정 하 고 데이터베이스의 변경 내용을 저장 한 다음 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-153">This code adds the Student entity created by the ASP.NET MVC model binder to the Students entity set and then saves the changes to the database.</span></span> <span data-ttu-id="102ac-154">(모델 바인더는 ASP.NET MVC 기능을 참조 하면 쉽게 폼에서 전송한 데이터에 사용할 수 있습니다, 모델 바인더는 CLR 형식에 게시 된 양식 값을 변환 하 고 동작 메서드 매개 변수에서 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-154">(Model binder refers to the ASP.NET MVC functionality that makes it easier for you to work with data submitted by a form; a model binder converts posted form values to CLR types and passes them to the action method in parameters.</span></span> <span data-ttu-id="102ac-155">이 경우, 모델 바인더를 폼 컬렉션에서 속성 값을 사용 하 여 학생 엔터티를 인스턴스화합니다.)</span><span class="sxs-lookup"><span data-stu-id="102ac-155">In this case, the model binder instantiates a Student entity for you using property values from the Form collection.)</span></span>

<span data-ttu-id="102ac-156">제거 하면 `ID` 에서 `Bind` 특성 ID가 있는 행이 삽입 될 때 SQL 서버가 자동으로 설정 하는 기본 키 값입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-156">You removed `ID` from the `Bind` attribute because ID is the primary key value which SQL Server will set automatically when the row is inserted.</span></span> <span data-ttu-id="102ac-157">사용자의 입력 ID 값을 설정 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-157">Input from the user does not set the ID value.</span></span>

<span data-ttu-id="102ac-158">이외의 `Bind` 특성 try catch 블록은 스 캐 폴드 코드에 대 한 유일한 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-158">Other than the `Bind` attribute, the try-catch block is the only change you've made to the scaffolded code.</span></span> <span data-ttu-id="102ac-159">예외에서 파생 되는 경우 `DbUpdateException` 는 변경 내용을 저장 하는 동안 발견 되었습니다, 일반 오류 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-159">If an exception that derives from `DbUpdateException` is caught while the changes are being saved, a generic error message is displayed.</span></span> <span data-ttu-id="102ac-160">`DbUpdateException`예외 경우에 따라 인해 외부 프로그래밍 오류가 아니라 응용 프로그램에 있는 하므로 사용자는 다시 시도 하는 데 권장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-160">`DbUpdateException` exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again.</span></span> <span data-ttu-id="102ac-161">이 샘플에서 구현 되지 않지만 프로덕션 품질 응용 프로그램은 예외를 기록 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-161">Although not implemented in this sample, a production quality application would log the exception.</span></span> <span data-ttu-id="102ac-162">자세한 내용은 참조는 **통찰력에 대 한 로그** 섹션 [모니터링 및 원격 분석 (실제 클라우드로 응용 프로그램 빌딩 Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-162">For more information, see the **Log for insight** section in [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).</span></span>

<span data-ttu-id="102ac-163">`ValidateAntiForgeryToken` 특성 교차 사이트 요청 위조 (CSRF) 공격을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-163">The `ValidateAntiForgeryToken` attribute helps prevent cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="102ac-164">토큰이 자동으로 주입 하 여 보기에는 [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) 사용자가 폼이 전송 될 때 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-164">The token is automatically injected into the view by the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) and is included when the form is submitted by the user.</span></span> <span data-ttu-id="102ac-165">토큰에 따라 유효성이 검사 된 `ValidateAntiForgeryToken` 특성입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-165">The token is validated by the `ValidateAntiForgeryToken` attribute.</span></span> <span data-ttu-id="102ac-166">CSRF에 대 한 자세한 내용은 참조 [바이러스 요청 위조](../../security/anti-request-forgery.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-166">For more information about CSRF, see [Anti-Request Forgery](../../security/anti-request-forgery.md).</span></span>

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a><span data-ttu-id="102ac-167">Overposting에 대 한 보안 정보</span><span class="sxs-lookup"><span data-stu-id="102ac-167">Security note about overposting</span></span>

<span data-ttu-id="102ac-168">`Bind` 스 캐 폴드 코드에 포함 된 특성은 `Create` 메서드는 한 가지 방법은 overposting 으로부터 보호 하기 위해에서 시나리오 만들기.</span><span class="sxs-lookup"><span data-stu-id="102ac-168">The `Bind` attribute that the scaffolded code includes on the `Create` method is one way to protect against overposting in create scenarios.</span></span> <span data-ttu-id="102ac-169">예를 들어, 학생 엔터티를 포함 한 `Secret` 속성을 설정 하려면이 웹 페이지를 표시 하지 않으려면입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-169">For example, suppose the Student entity includes a `Secret` property that you don't want this web page to set.</span></span>

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

<span data-ttu-id="102ac-170">없는 경우에 한 `Secret` 웹 페이지는 해커가 필드에서 Fiddler와 같은 도구를 사용 하거나 게시 하려면 일부 JavaScript 작성 한 `Secret` 값을 형성 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-170">Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="102ac-171">없이 `Bind` 하는 모델 바인더 모델 바인더 학생 인스턴스를 만들 때 사용 되는 필드를 제한 하는 특성은 선택 `Secret` 값 형태와 학생 엔터티 인스턴스를 만들려면 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-171">Without the `Bind` attribute limiting the fields that the model binder uses when it creates a Student instance, the model binder would pick up that `Secret` form value and use it to create the Student entity instance.</span></span> <span data-ttu-id="102ac-172">에 대해 지정 된 해커가 어떤 값이 다음는 `Secret` 양식 필드 데이터베이스에서 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-172">Then whatever value the hacker specified for the `Secret` form field would be updated in your database.</span></span> <span data-ttu-id="102ac-173">다음 이미지에서는 Fiddler 도구 추가 보여 줍니다.는 `Secret` 게시 된 양식 값을 필드 (값 "OverPost")을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-173">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![암호 필드를 추가 하는 fiddler](crud/_static/fiddler.png)

<span data-ttu-id="102ac-175">값 "OverPost" 다음 성공적으로에 추가 되는 `Secret` 속성 삽입 행을 해당 속성을 설정 하는 웹 페이지 수 있도록 의도 하지 않은 있지만 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-175">The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to set that property.</span></span>

<span data-ttu-id="102ac-176">먼저 데이터베이스에서 엔터티를 읽은 한 다음 호출 하 여 편집 시나리오에서 overposting를 방지할 수 있습니다 `TryUpdateModel`명시적 허용 되는 속성 목록에 전달 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-176">You can prevent overposting in edit scenarios by reading the entity from the database first and then calling `TryUpdateModel`, passing in an explicit allowed properties list.</span></span> <span data-ttu-id="102ac-177">이 자습서에 사용 되는 방법입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-177">That is the method used in these tutorials.</span></span>

<span data-ttu-id="102ac-178">대부분의 개발자가 기본 overposting 방지 하는 다른 방법으로 모델 바인딩으로 엔터티 클래스를 사용 하지 않고 모델 보기를 사용 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-178">An alternative way to prevent overposting that is preferred by many developers is to use view models rather than entity classes with model binding.</span></span> <span data-ttu-id="102ac-179">뷰 모델에서 업데이트 하려는 속성만 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-179">Include only the properties you want to update in the view model.</span></span> <span data-ttu-id="102ac-180">MVC 모델 바인더 완료 되 면 필요에 따라 AutoMapper 같은 도구를 사용 하 여 엔터티 인스턴스에 보기 모델 속성을 복사 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-180">Once the MVC model binder has finished, copy the view model properties to the entity instance, optionally using a tool such as AutoMapper.</span></span> <span data-ttu-id="102ac-181">사용 하 여 `_context.Entry` 해당 상태를 설정 하려면 엔터티 인스턴스에서 `Unchanged`를 설정한 `Property("PropertyName").IsModified` 의 보기 모델에 포함 된 각 엔터티 속성에서 true로 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-181">Use `_context.Entry` on the entity instance to set its state to `Unchanged`, and then set `Property("PropertyName").IsModified` to true on each entity property that is included in the view model.</span></span> <span data-ttu-id="102ac-182">이 메서드가 작동 모두에서 편집한 시나리오를 만들 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-182">This method works in both edit and create scenarios.</span></span>

### <a name="test-the-create-page"></a><span data-ttu-id="102ac-183">테스트 만들기 페이지</span><span class="sxs-lookup"><span data-stu-id="102ac-183">Test the Create page</span></span>

<span data-ttu-id="102ac-184">코드 *Views/Students/Create.cshtml* 사용 하 여 `label`, `input`, 및 `span` (유효성 검사 메시지의 경우)에 대 한 태그 각 필드에 대 한 도우미입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-184">The code in *Views/Students/Create.cshtml* uses `label`, `input`, and `span` (for validation messages) tag helpers for each field.</span></span>

<span data-ttu-id="102ac-185">응용 프로그램을 실행, 선택는 **학생** 탭을 클릭 **새로 만들기**합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-185">Run the app, select the **Students** tab, and click **Create New**.</span></span>

<span data-ttu-id="102ac-186">이름 및 날짜를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-186">Enter names and a date.</span></span> <span data-ttu-id="102ac-187">브라우저를 사용 하면 그렇게 하는 경우 잘못 된 날짜를 입력 하십시오.</span><span class="sxs-lookup"><span data-stu-id="102ac-187">Try entering an invalid date if your browser lets you do that.</span></span> <span data-ttu-id="102ac-188">(일부 브라우저 강제로 날짜 선택을 사용할 수 있습니다.) 클릭 **만들기** 오류 메시지를 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-188">(Some browsers force you to use a date picker.) Then click **Create** to see the error message.</span></span>

![날짜 유효성 검사 오류](crud/_static/date-error.png)

<span data-ttu-id="102ac-190">이 기본적으로 얻을 수 있는 서버 쪽 유효성 검사 이후의 자습서에서 또한 클라이언트 쪽 유효성 검사에 대 한 코드를 생성 하는 특성을 추가 하는 방법을 볼 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-190">This is server-side validation that you get by default; in a later tutorial you'll see how to add attributes that will generate code for client-side validation also.</span></span> <span data-ttu-id="102ac-191">다음 강조 표시 된 코드에서 모델 유효성 검사를 보여 줍니다.는 `Create` 메서드.</span><span class="sxs-lookup"><span data-stu-id="102ac-191">The following highlighted code shows the model validation check in the `Create` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

<span data-ttu-id="102ac-192">유효한 값으로 날짜를 변경 하 고 클릭 **만들기** 새 학생에 표시를 볼 수는 **인덱스** 페이지.</span><span class="sxs-lookup"><span data-stu-id="102ac-192">Change the date to a valid value and click **Create** to see the new student appear in the **Index** page.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="102ac-193">업데이트 편집 페이지</span><span class="sxs-lookup"><span data-stu-id="102ac-193">Update the Edit page</span></span>

<span data-ttu-id="102ac-194">*StudentController.cs*는 HttpGet `Edit` 메서드 (없이 하나는 `HttpPost` 특성) 사용 하 여는 `SingleOrDefaultAsync` 에서 본 것 처럼 선택한 학생 엔터티를 검색 하는 메서드는 `Details` 메서드.</span><span class="sxs-lookup"><span data-stu-id="102ac-194">In *StudentController.cs*, the HttpGet `Edit` method (the one without the `HttpPost` attribute) uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the `Details` method.</span></span> <span data-ttu-id="102ac-195">이 방법을 변경할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-195">You don't need to change this method.</span></span>

### <a name="recommended-httppost-edit-code-read-and-update"></a><span data-ttu-id="102ac-196">코드 HttpPost 편집 권장: 읽기 및 업데이트</span><span class="sxs-lookup"><span data-stu-id="102ac-196">Recommended HttpPost Edit code: Read and update</span></span>

<span data-ttu-id="102ac-197">HttpPost 편집 동작 메서드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-197">Replace the HttpPost Edit action method with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

<span data-ttu-id="102ac-198">이러한 변경 내용은 overposting 방지 하기 위해 보안 모범 사례를 구현 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-198">These changes implement a security best practice to prevent overposting.</span></span> <span data-ttu-id="102ac-199">생성 된 scaffolder는 `Bind` 특성을 사용 하 여 설정 엔터티를 모델 바인더에서 만든 엔터티를 추가 `Modified` 플래그입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-199">The scaffolder generated a `Bind` attribute and added the entity created by the model binder to the entity set with a `Modified` flag.</span></span> <span data-ttu-id="102ac-200">때문에 코드 대부분의 시나리오에 권장 되지 않는다고는 `Bind` 특성에 나열 되지 않은 필드에서 기존의 모든 데이터를 지웁니다는 `Include` 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-200">That code is not recommended for many scenarios because the `Bind` attribute clears out any pre-existing data in fields not listed in the `Include` parameter.</span></span>

<span data-ttu-id="102ac-201">기존 엔터티 및 호출에서 새 코드를 읽고 `TryUpdateModel` 필드 검색 된 엔터티를 업데이트 하 [폼 게시 된 데이터에서 사용자 입력에 따라](xref:mvc/models/model-binding#how-model-binding-works)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-201">The new code reads the existing entity and calls `TryUpdateModel` to update fields in the retrieved entity [based on user input in the posted form data](xref:mvc/models/model-binding#how-model-binding-works).</span></span> <span data-ttu-id="102ac-202">Entity Framework의 변경 내용 자동 추적 설정은 `Modified` 폼 입력에 의해 변경 된 필드에 대 한 플래그입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-202">The Entity Framework's automatic change tracking sets the `Modified` flag on the fields that are changed by form input.</span></span> <span data-ttu-id="102ac-203">경우는 `SaveChanges` 메서드가 호출 되 면 Entity Framework 데이터베이스 행을 업데이트 하는 SQL 문을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-203">When the `SaveChanges` method is called, the Entity Framework creates SQL statements to update the database row.</span></span> <span data-ttu-id="102ac-204">동시성 충돌 무시 되 고 데이터베이스에서 사용자가 업데이트 된 테이블 열만 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-204">Concurrency conflicts are ignored, and only the table columns that were updated by the user are updated in the database.</span></span> <span data-ttu-id="102ac-205">(이후의 자습서 동시성 충돌을 처리 하는 방법을 보여 줍니다.)</span><span class="sxs-lookup"><span data-stu-id="102ac-205">(A later tutorial shows how to handle concurrency conflicts.)</span></span>

<span data-ttu-id="102ac-206">초과 게시 하 여 업데이트할 수 있는 필드를 방지 하기 위해 가장 좋은 방법은 **편집** 페이지는 허용 목록에는 `TryUpdateModel` 매개 변수입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-206">As a best practice to prevent overposting, the fields that you want to be updateable by the **Edit** page are whitelisted in the `TryUpdateModel` parameters.</span></span> <span data-ttu-id="102ac-207">(매개 변수 목록에서 필드 목록 앞에 빈 문자열은 양식 필드 이름으로 사용 하는 접두사입니다.) 현재는 보호 하는 추가 필드가 없습니다 되지만 바인딩할 모델 바인더를 원하는 필드를 나열 하 여 데이터 모델에 나중에 필드 추가 하는 경우 자동으로 보호 되는지 명시적으로 추가 해야만 여기 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-207">(The empty string preceding the list of fields in the parameter list is for a prefix to use with the form fields names.) Currently there are no extra fields that you're protecting, but listing the fields that you want the model binder to bind ensures that if you add fields to the data model in the future, they're automatically protected until you explicitly add them here.</span></span>

<span data-ttu-id="102ac-208">이와 같이 변경,는 HttpPost의 메서드 서명에 `Edit` 는 HttpGet와 같습니다 `Edit` 메서드; 메서드 이름이 변경 했으므로 따라서 `EditPost`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-208">As a result of these changes, the method signature of the HttpPost `Edit` method is the same as the HttpGet `Edit` method; therefore you've renamed the method `EditPost`.</span></span>

### <a name="alternative-httppost-edit-code-create-and-attach"></a><span data-ttu-id="102ac-209">대체 HttpPost 편집 코드: 만들고 연결 하려면</span><span class="sxs-lookup"><span data-stu-id="102ac-209">Alternative HttpPost Edit code: Create and attach</span></span>

<span data-ttu-id="102ac-210">권장된 HttpPost 코드를 편집 하면 변경 된 열만 업데이트 되는 모델 바인딩에 대 한 포함 된 의도 하지 않은 속성에 데이터 유지.</span><span class="sxs-lookup"><span data-stu-id="102ac-210">The recommended HttpPost edit code ensures that only changed columns get updated and preserves data in properties that you don't want included for model binding.</span></span> <span data-ttu-id="102ac-211">그러나 읽기 우선 접근 방식은 해야 추가 데이터베이스 읽고 동시성 충돌을 처리 하기 위한 더 복잡 한 코드에서 발생할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-211">However, the read-first approach requires an extra database read, and can result in more complex code for handling concurrency conflicts.</span></span> <span data-ttu-id="102ac-212">해도 EF 컨텍스트와 모델 바인더를 통해 만든 엔터티를 연결 하 고 수정 된 것으로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-212">An alternative is to attach an entity created by the model binder to the EF context and mark it as modified.</span></span> <span data-ttu-id="102ac-213">(프로젝트를 업데이트 하지 않으면이 코드로 선택적 접근 방식을 설명 하기 위해 표시만 했습니다.)</span><span class="sxs-lookup"><span data-stu-id="102ac-213">(Don't update your project with this code, it's only shown to illustrate an optional approach.)</span></span> 

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

<span data-ttu-id="102ac-214">웹 페이지 UI 필드의 모든 엔터티를 포함 하 고 그 중 하나를 업데이트할 수 있는 경우이 방법을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-214">You can use this approach when the web page UI includes all of the fields in the entity and can update any of them.</span></span>

<span data-ttu-id="102ac-215">스 캐 폴드 코드 만들기 / 연결 방법을 사용 하 여 있지만 catch `DbUpdateConcurrencyException` 404 오류 코드를 예외 및 반환 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-215">The scaffolded code uses the create-and-attach approach but only catches `DbUpdateConcurrencyException` exceptions and returns 404 error codes.</span></span>  <span data-ttu-id="102ac-216">데이터베이스 업데이트 예외를 catch 하 고 오류 메시지를 표시 하는 표시 된 예제입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-216">The example shown catches any database update exception and displays an error message.</span></span>

### <a name="entity-states"></a><span data-ttu-id="102ac-217">엔터티 상태</span><span class="sxs-lookup"><span data-stu-id="102ac-217">Entity States</span></span>

<span data-ttu-id="102ac-218">메모리에 엔터티는 해당 행에는 데이터베이스와 동기화 하 고이 정보를 호출할 때 수행 되는 작업을 결정 하는 여부를 추적 데이터베이스 컨텍스트는 `SaveChanges` 메서드.</span><span class="sxs-lookup"><span data-stu-id="102ac-218">The database context keeps track of whether entities in memory are in sync with their corresponding rows in the database, and this information determines what happens when you call the `SaveChanges` method.</span></span> <span data-ttu-id="102ac-219">예를 들어 전달 하는 경우에 새 엔터티는 `Add` 메서드 엔터티의 상태로 설정 된 `Added`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-219">For example, when you pass a new entity to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="102ac-220">호출 하면는 `SaveChanges` 메서드, 데이터베이스 컨텍스트 SQL INSERT 명령을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-220">Then when you call the `SaveChanges` method, the database context issues a SQL INSERT command.</span></span>

<span data-ttu-id="102ac-221">엔터티 상태는 다음 중 하나일 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-221">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="102ac-222">`Added`.</span><span class="sxs-lookup"><span data-stu-id="102ac-222">`Added`.</span></span> <span data-ttu-id="102ac-223">엔터티는 데이터베이스에 아직 존재 하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-223">The entity does not yet exist in the database.</span></span> <span data-ttu-id="102ac-224">`SaveChanges` 메서드는 INSERT 문을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-224">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="102ac-225">`Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="102ac-225">`Unchanged`.</span></span> <span data-ttu-id="102ac-226">하 여이 엔터티를 사용 하 여 수행 해야 하는 아무 것도 `SaveChanges` 메서드.</span><span class="sxs-lookup"><span data-stu-id="102ac-226">Nothing needs to be done with this entity by the `SaveChanges` method.</span></span> <span data-ttu-id="102ac-227">데이터베이스에서 엔터티를 읽을 때 엔터티가이 상태에 있는 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-227">When you read an entity from the database, the entity starts out with this status.</span></span>

* <span data-ttu-id="102ac-228">`Modified`.</span><span class="sxs-lookup"><span data-stu-id="102ac-228">`Modified`.</span></span> <span data-ttu-id="102ac-229">엔터티의 속성 값의 일부 또는 모든 수정 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-229">Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="102ac-230">`SaveChanges` 메서드는 UPDATE 문을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-230">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="102ac-231">`Deleted`.</span><span class="sxs-lookup"><span data-stu-id="102ac-231">`Deleted`.</span></span> <span data-ttu-id="102ac-232">엔터티 삭제 하도록 표시 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-232">The entity has been marked for deletion.</span></span> <span data-ttu-id="102ac-233">`SaveChanges` 메서드 DELETE 문을 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-233">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="102ac-234">`Detached`.</span><span class="sxs-lookup"><span data-stu-id="102ac-234">`Detached`.</span></span> <span data-ttu-id="102ac-235">엔터티는 데이터베이스 컨텍스트에서 추적 되 고 있지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-235">The entity isn't being tracked by the database context.</span></span>

<span data-ttu-id="102ac-236">데스크톱 응용 프로그램 상태 변경 내용이 일반적으로 자동으로 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-236">In a desktop application, state changes are typically set automatically.</span></span> <span data-ttu-id="102ac-237">엔터티를 읽고 해당 속성 값의 일부 변경 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-237">You read an entity and make changes to some of its property values.</span></span> <span data-ttu-id="102ac-238">이렇게 하면 해당 엔터티 상태를 자동으로 변경 해야 `Modified`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-238">This causes its entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="102ac-239">호출 하면 `SaveChanges`, Entity Framework를 변경 하는 실제 속성만 업데이트 하는 SQL UPDATE 문을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-239">Then when you call `SaveChanges`, the Entity Framework generates a SQL UPDATE statement that updates only the actual properties that you changed.</span></span>

<span data-ttu-id="102ac-240">웹 앱의 경우에 `DbContext` 엔터티와 페이지를 렌더링 한 다음 해당 데이터를 편집할 수에 삭제 표시 처음에 읽는 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-240">In a web app, the `DbContext` that initially reads an entity and displays its data to be edited is disposed after a page is rendered.</span></span> <span data-ttu-id="102ac-241">경우는 HttpPost `Edit` 동작 메서드는, 새 웹 요청이 만들어지면 및의 새 인스턴스는 `DbContext`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-241">When the HttpPost `Edit` action method is called,  a new web request is made and you have a new instance of the `DbContext`.</span></span> <span data-ttu-id="102ac-242">새 해당 컨텍스트 내에서 엔터티를 다시 읽을 경우 데스크톱 처리를 시뮬레이션 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-242">If you re-read the entity in that new context, you simulate desktop processing.</span></span>

<span data-ttu-id="102ac-243">하지만 추가 읽기 작업을 수행 하지 않으려는 경우, 모델 바인더를 통해 생성 된 엔터티 개체를 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-243">But if you don't want to do the extra read operation, you have to use the entity object created by the model binder.</span></span>  <span data-ttu-id="102ac-244">이 작업을 수행 하는 가장 간단한 방법은 앞에서 살펴본 대체 HttpPost 편집 코드 에서처럼 엔터티 상태를 수정한 날짜를 설정 하려면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-244">The simplest way to do this is to set the entity state to Modified as is done in the alternative HttpPost Edit code shown earlier.</span></span> <span data-ttu-id="102ac-245">호출 하면 `SaveChanges`, Entity Framework는 컨텍스트는 변경 되는 속성을 알 수 없기 때문에 데이터베이스 행의 모든 열을 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-245">Then when you call `SaveChanges`, the Entity Framework updates all columns of the database row, because the context has no way to know which properties you changed.</span></span>

<span data-ttu-id="102ac-246">읽기 중심 접근 방식을 방지 하려면 표시 되지만 사용자가 실제로 변경 된 필드에만 업데이트 하도록 SQL UPDATE 문을 시겠습니까 코드는 복잡 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-246">If you want to avoid the read-first approach, but you also want the SQL UPDATE statement to update only the fields that the user actually changed, the code is more complex.</span></span> <span data-ttu-id="102ac-247">에 특정 한 방식으로 원래 값을 저장 해야 (같은 숨겨진된 필드를 사용 하 여)를 사용할 수 있는 경우는 HttpPost `Edit` 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-247">You have to save the original values in some way (such as by using hidden fields) so that they are available when the HttpPost `Edit` method is called.</span></span> <span data-ttu-id="102ac-248">그러면 원래 값이 호출을 사용 하 여 학생 엔터티를 만들 수 있습니다는 `Attach` 메서드는 해당 원래 버전은 엔터티의 엔터티의 값을 새 값으로 업데이트 한 다음 호출 `SaveChanges`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-248">Then you can create a Student entity using the original values, call the `Attach` method with that original version of the entity, update the entity's values to the new values, and then call `SaveChanges`.</span></span>

### <a name="test-the-edit-page"></a><span data-ttu-id="102ac-249">편집 페이지를 테스트 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-249">Test the Edit page</span></span>

<span data-ttu-id="102ac-250">응용 프로그램을 실행, 선택는 **학생** 탭을 클릭 한 다음는 **편집** 하이퍼링크입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-250">Run the app, select the **Students** tab, then click an **Edit** hyperlink.</span></span>

![학생 편집 페이지](crud/_static/student-edit.png)

<span data-ttu-id="102ac-252">클릭 하 여 확인 하 고 데이터의 일부 변경 **저장**합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-252">Change some of the data and click **Save**.</span></span> <span data-ttu-id="102ac-253">**인덱스** 페이지가 열리고 변경 된 데이터를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-253">The **Index** page opens and you see the changed data.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="102ac-254">업데이트 페이지 삭제</span><span class="sxs-lookup"><span data-stu-id="102ac-254">Update the Delete page</span></span>

<span data-ttu-id="102ac-255">*StudentController.cs*, 템플릿 코드는 HttpGet `Delete` 메서드는 `SingleOrDefaultAsync` 메서드 세부 정보 및 편집에서 볼 수 있듯이 선택한 학생 엔터티를 검색 하는 메서드입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-255">In *StudentController.cs*, the template code for the HttpGet `Delete` method uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the Details and Edit methods.</span></span> <span data-ttu-id="102ac-256">그러나을 구현 하는 사용자 지정 오류 메시지에 대 한 호출 `SaveChanges` 실패 하면이 메서드를 해당 보기의 일부 기능을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-256">However, to implement a custom error message when the call to `SaveChanges` fails, you'll add some functionality to this method and its corresponding view.</span></span>

<span data-ttu-id="102ac-257">업데이트에 대 한 준다는 사실을 알았습니다 하 고 작업을 만드는 두 개의 작업 메서드 삭제 작업에 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-257">As you saw for update and create operations, delete operations require two action methods.</span></span> <span data-ttu-id="102ac-258">GET 요청에 대 한 응답에서 호출 되는 메서드를 승인 하거나 삭제 작업을 취소 하는 사용자에 게 제공 하는 보기를 표시 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-258">The method that is called in response to a GET request displays a view that gives the user a chance to approve or cancel the delete operation.</span></span> <span data-ttu-id="102ac-259">사용자가 승인 하는 경우 POST 요청 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-259">If the user approves it, a POST request is created.</span></span> <span data-ttu-id="102ac-260">이런 경우, 고 HttpPost `Delete` 메서드가 호출 되 고 실제로 해당 메서드에 삭제 동작을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-260">When that happens, the HttpPost `Delete` method is called and then that method actually performs the delete operation.</span></span>

<span data-ttu-id="102ac-261">Try catch 블록에서 HttpPost에 추가할 `Delete` 데이터베이스를 업데이트할 때 발생할 수 있는 오류를 처리 하는 메서드.</span><span class="sxs-lookup"><span data-stu-id="102ac-261">You'll add a try-catch block to the HttpPost `Delete` method to handle any errors that might occur when the database is updated.</span></span> <span data-ttu-id="102ac-262">오류가 발생 하면 HttpPost Delete 메서드는 오류가 발생 했음을 나타내는 매개 변수 전달 HttpGet Delete 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-262">If an error occurs, the HttpPost Delete method calls the HttpGet Delete method, passing it a parameter that indicates that an error has occurred.</span></span> <span data-ttu-id="102ac-263">그런 다음 HttpGet Delete 메서드는 기회를 취소 하거나 다시 시도 하십시오. 사용자에 게 제공 하는 오류 메시지와 함께 확인 페이지 다시 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-263">The HttpGet Delete method then redisplays the confirmation page along with the error message, giving the user an opportunity to cancel or try again.</span></span>

<span data-ttu-id="102ac-264">교체는 HttpGet `Delete` 동작 메서드를 다음 코드로, 오류 보고를 관리 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-264">Replace the HttpGet `Delete` action method with the following code, which manages error reporting.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

<span data-ttu-id="102ac-265">이 코드는 변경 내용을 저장 하려면 오류 발생 후 메서드가 호출 된 있는지 여부를 나타내는 선택적 매개 변수를 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-265">This code accepts an optional parameter that indicates whether the method was called after a failure to save changes.</span></span> <span data-ttu-id="102ac-266">이 매개 변수는 false는 HttpGet `Delete` 이전에 실패 한 없이 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-266">This parameter is false when the HttpGet `Delete` method is called without a previous failure.</span></span> <span data-ttu-id="102ac-267">HttpPost에서 호출 될 때 `Delete` 데이터베이스 업데이트 오류에 대 한 응답에서 메서드 매개 변수는 true 및 보기에 오류 메시지가 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-267">When it is called by the HttpPost `Delete` method in response to a database update error, the parameter is true and an error message is passed to the view.</span></span>

### <a name="the-read-first-approach-to-httppost-delete"></a><span data-ttu-id="102ac-268">HttpPost 삭제에 대 한 읽기 중심 접근</span><span class="sxs-lookup"><span data-stu-id="102ac-268">The read-first approach to HttpPost Delete</span></span>

<span data-ttu-id="102ac-269">대체는 HttpPost `Delete` 동작 메서드 (라는 `DeleteConfirmed`)를 다음 코드로 실제 삭제 동작을 수행 하 고이 한 데이터베이스 업데이트 오류를 catch 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-269">Replace the HttpPost `Delete` action method (named `DeleteConfirmed`) with the following code, which performs the actual delete operation and catches any database update errors.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

<span data-ttu-id="102ac-270">이 코드에서는 선택된 된 엔터티를 검색 한 다음 호출는 `Remove` 엔터티의 상태 설정 하는 방법은 `Deleted`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-270">This code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="102ac-271">때 `SaveChanges` 호출, SQL DELETE 명령이 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-271">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span>

### <a name="the-create-and-attach-approach-to-httppost-delete"></a><span data-ttu-id="102ac-272">HttpPost 삭제 하는 만들기 / 연결 방법</span><span class="sxs-lookup"><span data-stu-id="102ac-272">The create-and-attach approach to HttpPost Delete</span></span>

<span data-ttu-id="102ac-273">주를 사용 하 여 학생 엔터티를 인스턴스화하여 불필요 한 SQL 쿼리를 방지할 수는 대규모 응용 프로그램의 성능을 개선 우선 순위 이면 키 값 및 다음 엔터티 상태를 설정 하는 것 `Deleted`합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-273">If improving performance in a high-volume application is a priority, you could avoid an unnecessary SQL query by instantiating a Student entity using only the primary key value and then setting the entity state to `Deleted`.</span></span> <span data-ttu-id="102ac-274">Entity Framework는 엔터티를 삭제 하는 데 필요한 모든입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-274">That's all that the Entity Framework needs in order to delete the entity.</span></span> <span data-ttu-id="102ac-275">(프로젝트에이 코드를 배치 하지 마십시오; 대신을 보여 주기 위해 여기는 합니다.)</span><span class="sxs-lookup"><span data-stu-id="102ac-275">(Don't put this code in your project; it's here just to illustrate an alternative.)</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

<span data-ttu-id="102ac-276">엔터티도 삭제 해야 하는 데이터 관련에 cascade delete 해당 데이터베이스에 구성 되어 있는지 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-276">If the entity has related data that should also be deleted, make sure that cascade delete is configured in the database.</span></span> <span data-ttu-id="102ac-277">엔터티 삭제에이 접근 방식 EF 사실을 모를 수 관련 엔터티가 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-277">With this approach to entity deletion, EF might not realize there are related entities to be deleted.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="102ac-278">삭제 뷰 업데이트</span><span class="sxs-lookup"><span data-stu-id="102ac-278">Update the Delete view</span></span>

<span data-ttu-id="102ac-279">*Views/Student/Delete.cshtml*, 다음 예제와 같이 h2 제목 및 h3 머리글 사이 오류 메시지가 추가:</span><span class="sxs-lookup"><span data-stu-id="102ac-279">In *Views/Student/Delete.cshtml*, add an error message between the h2 heading and the h3 heading, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

<span data-ttu-id="102ac-280">응용 프로그램을 실행, 선택는 **학생** 탭을 클릭 한 **삭제** 하이퍼링크:</span><span class="sxs-lookup"><span data-stu-id="102ac-280">Run the app, select the **Students** tab, and click a **Delete** hyperlink:</span></span>

![삭제 확인 페이지](crud/_static/student-delete.png)

<span data-ttu-id="102ac-282">클릭 **삭제**합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-282">Click **Delete**.</span></span> <span data-ttu-id="102ac-283">인덱스 페이지 삭제 된 학생 없이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-283">The Index page is displayed without the deleted student.</span></span> <span data-ttu-id="102ac-284">(오류 처리 코드 동시성 자습서에서 실제 동작에서의 예를 볼 수 있습니다.)</span><span class="sxs-lookup"><span data-stu-id="102ac-284">(You'll see an example of the error handling code in action in the concurrency tutorial.)</span></span>

## <a name="closing-database-connections"></a><span data-ttu-id="102ac-285">데이터베이스 연결 닫기</span><span class="sxs-lookup"><span data-stu-id="102ac-285">Closing database connections</span></span>

<span data-ttu-id="102ac-286">데이터베이스 연결을 보유 하는 리소스를 확보 하려면 컨텍스트 인스턴스가 삭제 해야 최대한 빨리을 마쳤습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-286">To free up the resources that a database connection holds, the context instance must be disposed as soon as possible when you are done with it.</span></span> <span data-ttu-id="102ac-287">ASP.NET Core 기본 제공 [종속성 주입](../../fundamentals/dependency-injection.md) 해당 작업을 담당 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-287">The ASP.NET Core built-in [dependency injection](../../fundamentals/dependency-injection.md) takes care of that task for you.</span></span>

<span data-ttu-id="102ac-288">*Startup.cs*, 호출 하는 [AddDbContext 확장 메서드](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) 프로 비전 하는 `DbContext` ASP.NET DI 컨테이너에는 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-288">In *Startup.cs*, you call the [AddDbContext extension method](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) to provision the `DbContext` class in the ASP.NET DI container.</span></span> <span data-ttu-id="102ac-289">메서드를 서비스 수명을 설정 `Scoped` 기본적으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-289">That method sets the service lifetime to `Scoped` by default.</span></span> <span data-ttu-id="102ac-290">`Scoped`컨텍스트 개체 수명 웹 요청 라이프 시간과 일치 하는 방법 및 `Dispose` 메서드가 웹 요청이 끝날 때 자동으로 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-290">`Scoped` means the context object lifetime coincides with the web request life time, and the `Dispose` method will be called automatically at the end of the web request.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="102ac-291">트랜잭션 처리</span><span class="sxs-lookup"><span data-stu-id="102ac-291">Handling Transactions</span></span>

<span data-ttu-id="102ac-292">기본적으로 Entity Framework는 트랜잭션을 암시적으로 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-292">By default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="102ac-293">여러 행 이나 테이블을 변경 하 고 호출 하는 다음 시나리오에서 `SaveChanges`, Entity Framework 하는지 자동으로 확인 중 하나 변경 내용을 모두 성공 하거나 모두 실패 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-293">In scenarios where you make changes to multiple rows or tables and then call `SaveChanges`, the Entity Framework automatically makes sure that either all of your changes succeed or they all fail.</span></span> <span data-ttu-id="102ac-294">일부 변경 내용이 먼저 완료 되는 경우 오류가 발생 하는 다음 해당 변경 내용이 자동으로 롤백됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-294">If some changes are done first and then an error happens, those changes are automatically rolled back.</span></span> <span data-ttu-id="102ac-295">여기서 필요한 세부적으로 제어할 수-예를 들어 트랜잭션에서-Entity Framework 밖에 서 수행 하는 작업을 포함 하도록 하려는 경우 시나리오 참조 [트랜잭션을](https://docs.microsoft.com/ef/core/saving/transactions)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-295">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="102ac-296">No 추적 쿼리</span><span class="sxs-lookup"><span data-stu-id="102ac-296">No-tracking queries</span></span>

<span data-ttu-id="102ac-297">데이터베이스 컨텍스트 테이블 행을 검색 하 고을 나타내는 엔터티 개체를 만드는 때 기본적으로를 추적 데이터베이스에 포함 된 내용으로 메모리에 엔터티 동기화 되었는지 여부입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-297">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="102ac-298">데이터를 메모리에에서 한 캐시 역할을 하 고 엔터티를 업데이트할 때 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-298">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="102ac-299">이 캐싱은 종종에 필요 하지 않은 웹 응용 프로그램 컨텍스트는 일반적으로 수명이 짧은 (새로운 삭제을 만들어 각 요청에 대 한) 인스턴스와 컨텍스트 때문에 읽는 엔터티는 해당 엔터티를 다시 사용 하기 전에 일반적으로 삭제 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-299">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="102ac-300">호출 하 여 메모리에 엔터티 개체의 추적을 해제할 수 있습니다는 `AsNoTracking` 메서드.</span><span class="sxs-lookup"><span data-stu-id="102ac-300">You can disable tracking of entity objects in memory by calling the `AsNoTracking` method.</span></span> <span data-ttu-id="102ac-301">다음과 같은 일반적인 시나리오를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-301">Typical scenarios in which you might want to do that include the following:</span></span>

* <span data-ttu-id="102ac-302">컨텍스트 수명 동안 않아도 모든 엔터티를 업데이트 하 고 EF에 필요 하지 않습니다 [별도 쿼리에 의해 검색 된 엔터티가 포함 된 탐색 속성을 자동으로 로드](read-related-data.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-302">During the context lifetime you don't need to update any entities, and you don't need EF to [automatically load navigation properties with  entities retrieved by separate queries](read-related-data.md).</span></span> <span data-ttu-id="102ac-303">자주 HttpGet 동작 메서드에서 컨트롤러의 이러한 조건이 충족 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-303">Frequently these conditions are met in a controller's HttpGet action methods.</span></span>

* <span data-ttu-id="102ac-304">많은 양의 데이터를 검색 하는 쿼리를 실행 하는 및 반환된 된 데이터의 작은 부분만 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-304">You are running a query that retrieves a large volume of data, and only a small portion of the returned data will be updated.</span></span> <span data-ttu-id="102ac-305">보다 효과적으로 큰 쿼리에 대해 추적을 해제 하 고 나중에 업데이트 해야 하는 몇 가지 엔터티에 대 한 쿼리를 실행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-305">It may be more efficient to turn off tracking for the large query, and run a query later for the few entities that need to be updated.</span></span>

* <span data-ttu-id="102ac-306">업데이트 하려면 엔터티를 연결 하려고 하지만 서로 다른 목적을 위해 동일한 엔터티를 검색할 이전 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-306">You want to attach an entity in order to update it, but earlier you retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="102ac-307">엔터티는 데이터베이스 컨텍스트에서 이미 추적 중인를 변경 하려면 해당 하는 엔터티를 연결할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-307">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="102ac-308">호출 하는 것이 상황을 처리 하는 한 가지 방법은 `AsNoTracking` 이전 쿼리 합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-308">One way to handle this situation is to call `AsNoTracking` on the earlier query.</span></span>

<span data-ttu-id="102ac-309">자세한 내용은 참조 [vs를 추적 합니다. No 추적](https://docs.microsoft.com/ef/core/querying/tracking)합니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-309">For more information, see [Tracking vs. No-Tracking](https://docs.microsoft.com/ef/core/querying/tracking).</span></span>

## <a name="summary"></a><span data-ttu-id="102ac-310">요약</span><span class="sxs-lookup"><span data-stu-id="102ac-310">Summary</span></span>

<span data-ttu-id="102ac-311">학생 엔터티에 대 한 간단한 CRUD 작업을 수행 하는 페이지에 대 한 전체 집합이 생깁니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-311">You now have a complete set of pages that perform simple CRUD operations for Student entities.</span></span> <span data-ttu-id="102ac-312">다음 자습서에서의 기능을 확장 합니다는 **인덱스** 정렬, 필터링 및 페이징을 추가 하 여 페이지입니다.</span><span class="sxs-lookup"><span data-stu-id="102ac-312">In the next tutorial you'll expand the functionality of the **Index** page by adding sorting, filtering, and paging.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="102ac-313">[이전](intro.md)
[다음](sort-filter-page.md)</span><span class="sxs-lookup"><span data-stu-id="102ac-313">[Previous](intro.md)
[Next](sort-filter-page.md)</span></span>  