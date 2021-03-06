---
title: ASP.NET Core에서 Web API 빌드
author: scottaddie
description: ASP.NET Core에서 Web API를 빌드하는 데 사용할 수 있는 기능과 각 기능을 사용하기에 적합한 시기에 대해 알아봅니다.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/15/2018
uid: web-api/index
ms.openlocfilehash: d410f28ff7fda3bf33f73c06b3e626dfd4ee7dd8
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2018
ms.locfileid: "41822142"
---
# <a name="build-web-apis-with-aspnet-core"></a>ASP.NET Core에서 Web API 빌드

작성자: [Scott Addie](https://github.com/scottaddie)

[예제 코드 살펴보기 및 다운로드](https://github.com/aspnet/Docs/tree/master/aspnetcore/web-api/define-controller/samples)([다운로드 방법](xref:tutorials/index#how-to-download-a-sample))

이 문서에서는 ASP.NET Core에서 Web API를 빌드하는 방법과 각 기능을 사용하기에 가장 적합한 시기에 대해 설명합니다.

## <a name="derive-class-from-controllerbase"></a>ControllerBase에서 클래스 파생

Web API를 제공할 목적으로 컨트롤의 <xref:Microsoft.AspNetCore.Mvc.ControllerBase> 클래스에서 상속합니다. 예:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_PetsController&highlight=3)]

::: moniker-end

`ControllerBase` 클래스는 여러 속성 및 메서드에 대한 액세스를 제공합니다. 위의 코드에 나온 예제에는 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.BadRequest(Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)> 및 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction(System.String,System.Object,System.Object)>가 포함되어 있습니다. 이러한 메서드는 각각 HTTP 400와 201 상태 코드를 반환하는 작업 메서드 내에서 호출됩니다. 요청 모델 유효성 검사를 처리하기 위해 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState> 속성에 액세스하며, 이 속성은 `ControllerBase`에서도 제공합니다.

::: moniker range=">= aspnetcore-2.1"

## <a name="annotate-class-with-apicontrollerattribute"></a>ApiControllerAttribute로 클래스에 주석 달기

ASP.NET Core 2.1에서는 Web API 컨트롤러 클래스를 나타내는 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 특성을 소개합니다. 예:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=2)]

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*>을 통해 설정된 호환성 버전 2.1 이상은 이 특성을 사용해야 합니다. 예를 들어 *Startup.ConfigureServices*에서 강조 표시된 코드는 2.1 호환성 플래그를 설정합니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_SetCompatibilityVersion&highlight=2)]

자세한 내용은 <xref:mvc/compatibility-version>을 참조하세요.

`[ApiController]` 특성은 일반적으로 `ControllerBase`와 함께 컨트롤러에 대한 REST 특정 동작을 사용하도록 설정합니다. `ControllerBase`는 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound*> 및 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.File*> 같은 메서드에 대한 액세스를 제공합니다.

다른 방법은 `[ApiController]` 특성으로 주석 지정된 사용자 지정 기본 컨트롤러 클래스를 만드는 것입니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/MyBaseController.cs?name=snippet_ControllerSignature)]

다음 섹션에서는 특성으로 추가된 편리한 기능을 설명합니다.

### <a name="automatic-http-400-responses"></a>자동 HTTP 400 응답

유효성 검사 오류 시 HTTP 400 응답이 자동으로 트리거됩니다. 다음 코드는 실제 작업 시 불필요하게 됩니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api.Pre21/Controllers/PetsController.cs?name=snippet_ModelStateIsValidCheck)]

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressModelStateInvalidFilter> 속성이 `true`로 설정된 경우 기본 동작이 사용되지 않습니다. `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 뒤에 *Startup.ConfigureServices*에서 다음 코드를 추가합니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=5)]

### <a name="binding-source-parameter-inference"></a>바인딩 소스 매개 변수 유추

바인딩 소스 특성은 작업 매개 변수 값이 발견되는 위치를 정의합니다. 다음 바인딩 소스 특성이 존재합니다.

|특성|바인딩 원본 |
|---------|---------|
|**[[FromBody]](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute)**     | 요청 본문 |
|**[[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute)**     | 요청 본문에서 양식 데이터 |
|**[[FromHeader]](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute)** | 요청 헤더 |
|**[[FromQuery]](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute)**   | 요청 쿼리 문자열 매개 변수 |
|**[[FromRoute]](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute)**   | 현재 요청의 경로 데이터 |
|**[[FromServices]](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices)** | 작업 매개 변수로 삽입된 요청 서비스 |

> [!WARNING]
> 값에 `%2f`(즉, `/`)이 포함될 수 있는 경우 `[FromRoute]`를 사용하지 않습니다. `%2f`는 `/`로 이스케이프가 해제되지 않습니다. 값에 `%2f`가 포함될 수 있으면 `[FromQuery]`를 사용합니다.

`[ApiController]` 특성 없이, 바인딩 소스 특성이 명시적으로 정의됩니다. 다음 예제에서 `[FromQuery]` 특성은 `discontinuedOnly` 매개 변수 값이 요청 URL의 쿼리 문자열에 제공됨을 나타냅니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_BindingSourceAttributes&highlight=3)]

유추 규칙이 작업 매개 변수의 기본 데이터 원본에 대해 적용됩니다. 이러한 규칙은 작업 매개 변수에 달리 수동으로 적용할 가능성이 높은 바인딩 소스를 구성합니다. 바인딩 소스 특성은 다음과 같이 동작합니다.

* **[FromBody]** 는 복합 형식 매개 변수에 대해 유추됩니다. 이 규칙은 <xref:Microsoft.AspNetCore.Http.IFormCollection> 및 <xref:System.Threading.CancellationToken> 같이 특별한 의미를 지닌 복합 기본 제공 형식에는 적용되지 않습니다. 바인딩 소스 유추 코드는 이러한 특별한 형식을 무시합니다. `[FromBody]`는 `string` 또는 `int` 같은 단순 형식에 대해 유추되지 않습니다. 따라서 해당 기능을 원하는 경우 단순 형식에 `[FromBody]` 특성을 사용해야 합니다. 작업에 명시적으로 지정(`[FromBody]`를 통해)되거나 요청 본문의 바인딩으로 유추된 한 개를 초과하는 매개 변수가 있는 경우 예외가 throw됩니다. 예를 들어 다음 작업 서명으로 예외가 발생합니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/TestController.cs?name=snippet_ActionsCausingExceptions)]

* **[FromForm]** 은 <xref:Microsoft.AspNetCore.Http.IFormFile> 및 <xref:Microsoft.AspNetCore.Http.IFormFileCollection> 형식의 작업 매개 변수에 대해 유추됩니다. 단순 또는 사용자 정의 형식에 대해서는 유추되지 않습니다.
* **[FromRoute]** 는 경로 템플릿에서 매개 변수와 일치하는 작업 매개 변수 이름에 대해 유추됩니다. 둘 이상의 경로가 작업 매개 변수와 일치하는 경우 모든 경로 값은 `[FromRoute]`로 간주됩니다.
* **[FromQuery]** 는 다른 작업 매개 변수에 대해 유추됩니다.

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressInferBindingSourcesForParameters> 속성이 `true`로 설정된 경우 기본 유추 규칙이 사용되지 않습니다. `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 뒤에 *Startup.ConfigureServices*에서 다음 코드를 추가합니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=4)]

### <a name="multipartform-data-request-inference"></a>다중 파트/폼 데이터 요청 유추

작업 매개 변수를 [[FromForm]](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) 특성으로 주석 처리하는 경우 `multipart/form-data` 요청 콘텐츠 형식이 유추됩니다.

<xref:Microsoft.AspNetCore.Mvc.ApiBehaviorOptions.SuppressConsumesConstraintForFormFileParameters> 속성이 `true`로 설정된 경우 기본 동작이 사용되지 않습니다. `services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);` 뒤에 *Startup.ConfigureServices*에서 다음 코드를 추가합니다.

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Startup.cs?name=snippet_ConfigureApiBehaviorOptions&highlight=3)]

### <a name="attribute-routing-requirement"></a>특성 라우팅 요구 사항

특성 라우팅은 요구 사항이 됩니다. 예:

[!code-csharp[](../web-api/define-controller/samples/WebApiSample.Api/Controllers/ProductsController.cs?name=snippet_ControllerSignature&highlight=1)]

작업은 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>에 정의된 [규칙 기반 경로](xref:mvc/controllers/routing#conventional-routing)를 통해 또는 *Startup.Configure*의 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*>에 의해 액세스할 수 없습니다.

::: moniker-end

## <a name="additional-resources"></a>추가 자료

* <xref:web-api/action-return-types>
* <xref:web-api/advanced/custom-formatters>
* <xref:web-api/advanced/formatting>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:mvc/controllers/routing>
