---
title: ASP.NET Core Blazor 이벤트 처리
author: guardrex
description: 이벤트 인수 형식, 이벤트 콜백, 기본 브라우저 이벤트 관리를 비롯한 Blazor의 이벤트 처리 기능에 대해 알아봅니다.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/16/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/event-handling
ms.openlocfilehash: 610cb9124f59ed07f1fe6193f92052b4513450c8
ms.sourcegitcommit: 69e1a79a572b0af17d08e81af12c594b7316f2e1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/15/2020
ms.locfileid: "83424245"
---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="3d10f-103">ASP.NET Core Blazor 이벤트 처리</span><span class="sxs-lookup"><span data-stu-id="3d10f-103">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="3d10f-104">작성자: [Luke Latham](https://github.com/guardrex) 및 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="3d10f-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="3d10f-105">Razor 구성 요소는 이벤트 처리 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-105">Razor components provide event handling features.</span></span> <span data-ttu-id="3d10f-106">대리자 형식 값을 사용하는 [`@on{EVENT}`](xref:mvc/views/razor#onevent)(예: `@onclick`)라는 HTML 요소 특성의 경우 Razor 구성 요소는 특성 값을 이벤트 처리기로 취급합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-106">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="3d10f-107">다음 코드는 UI에서 단추를 선택할 때 `UpdateHeading` 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-107">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="3d10f-108">다음 코드는 UI에서 확인란을 변경할 때 `CheckChanged` 메서드를 호출합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-108">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="3d10f-109">이벤트 처리기는 비동기로, <xref:System.Threading.Tasks.Task>를 반환할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-109">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="3d10f-110">[StateHasChanged](xref:blazor/lifecycle#state-changes)를 수동으로 호출할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-110">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="3d10f-111">예외가 발생하면 기록됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-111">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="3d10f-112">다음 예제에서는 단추를 선택할 때 `UpdateHeading`이 비동기적으로 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-112">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

## <a name="event-argument-types"></a><span data-ttu-id="3d10f-113">이벤트 인수 형식</span><span class="sxs-lookup"><span data-stu-id="3d10f-113">Event argument types</span></span>

<span data-ttu-id="3d10f-114">일부 이벤트의 경우 이벤트 인수 형식이 허용됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-114">For some events, event argument types are permitted.</span></span> <span data-ttu-id="3d10f-115">메서드에 이벤트 형식을 사용하는 경우에만 메서드 호출에 이벤트 형식을 지정하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-115">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="3d10f-116">지원되는 `EventArgs`는 다음 표에 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-116">Supported `EventArgs` are shown in the following table.</span></span>

| <span data-ttu-id="3d10f-117">이벤트</span><span class="sxs-lookup"><span data-stu-id="3d10f-117">Event</span></span>            | <span data-ttu-id="3d10f-118">클래스</span><span class="sxs-lookup"><span data-stu-id="3d10f-118">Class</span></span>                | <span data-ttu-id="3d10f-119">DOM 이벤트 및 참고 사항</span><span class="sxs-lookup"><span data-stu-id="3d10f-119">DOM events and notes</span></span> |
| ---------------- | -------------------- | -------------------- |
| <span data-ttu-id="3d10f-120">클립보드</span><span class="sxs-lookup"><span data-stu-id="3d10f-120">Clipboard</span></span>        | `ClipboardEventArgs` | <span data-ttu-id="3d10f-121">`oncut`, `oncopy`, `onpaste`</span><span class="sxs-lookup"><span data-stu-id="3d10f-121">`oncut`, `oncopy`, `onpaste`</span></span> |
| <span data-ttu-id="3d10f-122">끌기</span><span class="sxs-lookup"><span data-stu-id="3d10f-122">Drag</span></span>             | `DragEventArgs`      | <span data-ttu-id="3d10f-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span><span class="sxs-lookup"><span data-stu-id="3d10f-123">`ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="3d10f-124">`DataTransfer` 및 `DataTransferItem`은 끌어온 항목 데이터를 포함합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-124">`DataTransfer` and `DataTransferItem` hold dragged item data.</span></span> |
| <span data-ttu-id="3d10f-125">Error</span><span class="sxs-lookup"><span data-stu-id="3d10f-125">Error</span></span>            | `ErrorEventArgs`     | `onerror` |
| <span data-ttu-id="3d10f-126">이벤트</span><span class="sxs-lookup"><span data-stu-id="3d10f-126">Event</span></span>            | `EventArgs`          | <span data-ttu-id="3d10f-127">*일반*</span><span class="sxs-lookup"><span data-stu-id="3d10f-127">*General*</span></span><br><span data-ttu-id="3d10f-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="3d10f-128">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="3d10f-129">*클립보드*</span><span class="sxs-lookup"><span data-stu-id="3d10f-129">*Clipboard*</span></span><br><span data-ttu-id="3d10f-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="3d10f-130">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="3d10f-131">*입력*</span><span class="sxs-lookup"><span data-stu-id="3d10f-131">*Input*</span></span><br><span data-ttu-id="3d10f-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span><span class="sxs-lookup"><span data-stu-id="3d10f-132">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, `onsubmit`</span></span><br><br><span data-ttu-id="3d10f-133">*미디어*</span><span class="sxs-lookup"><span data-stu-id="3d10f-133">*Media*</span></span><br><span data-ttu-id="3d10f-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="3d10f-134">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span> |
| <span data-ttu-id="3d10f-135">포커스</span><span class="sxs-lookup"><span data-stu-id="3d10f-135">Focus</span></span>            | `FocusEventArgs`     | <span data-ttu-id="3d10f-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span><span class="sxs-lookup"><span data-stu-id="3d10f-136">`onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="3d10f-137">`relatedTarget` 지원을 포함하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-137">Doesn't include support for `relatedTarget`.</span></span> |
| <span data-ttu-id="3d10f-138">입력</span><span class="sxs-lookup"><span data-stu-id="3d10f-138">Input</span></span>            | `ChangeEventArgs`    | <span data-ttu-id="3d10f-139">`onchange`, `oninput`</span><span class="sxs-lookup"><span data-stu-id="3d10f-139">`onchange`, `oninput`</span></span> |
| <span data-ttu-id="3d10f-140">키보드</span><span class="sxs-lookup"><span data-stu-id="3d10f-140">Keyboard</span></span>         | `KeyboardEventArgs`  | <span data-ttu-id="3d10f-141">`onkeydown`, `onkeypress`, `onkeyup`</span><span class="sxs-lookup"><span data-stu-id="3d10f-141">`onkeydown`, `onkeypress`, `onkeyup`</span></span> |
| <span data-ttu-id="3d10f-142">마우스</span><span class="sxs-lookup"><span data-stu-id="3d10f-142">Mouse</span></span>            | `MouseEventArgs`     | <span data-ttu-id="3d10f-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span><span class="sxs-lookup"><span data-stu-id="3d10f-143">`onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout`</span></span> |
| <span data-ttu-id="3d10f-144">마우스 포인터</span><span class="sxs-lookup"><span data-stu-id="3d10f-144">Mouse pointer</span></span>    | `PointerEventArgs`   | <span data-ttu-id="3d10f-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span><span class="sxs-lookup"><span data-stu-id="3d10f-145">`onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture`</span></span> |
| <span data-ttu-id="3d10f-146">마우스 휠</span><span class="sxs-lookup"><span data-stu-id="3d10f-146">Mouse wheel</span></span>      | `WheelEventArgs`     | <span data-ttu-id="3d10f-147">`onwheel`, `onmousewheel`</span><span class="sxs-lookup"><span data-stu-id="3d10f-147">`onwheel`, `onmousewheel`</span></span> |
| <span data-ttu-id="3d10f-148">진행률</span><span class="sxs-lookup"><span data-stu-id="3d10f-148">Progress</span></span>         | `ProgressEventArgs`  | <span data-ttu-id="3d10f-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span><span class="sxs-lookup"><span data-stu-id="3d10f-149">`onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout`</span></span> |
| <span data-ttu-id="3d10f-150">터치</span><span class="sxs-lookup"><span data-stu-id="3d10f-150">Touch</span></span>            | `TouchEventArgs`     | <span data-ttu-id="3d10f-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="3d10f-151">`ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="3d10f-152">`TouchPoint`는 터치 인식 디바이스에서 단일 접촉 지점을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-152">`TouchPoint` represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="3d10f-153">자세한 내용은 다음 자료를 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="3d10f-153">For more information, see the following resources:</span></span>

* <span data-ttu-id="3d10f-154">[ASP.NET Core 참조 소스(dotnet/aspnetcore release/3.1 branch)의 EventArgs 클래스](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span><span class="sxs-lookup"><span data-stu-id="3d10f-154">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="3d10f-155">[MDN 웹 문서: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) - 각 DOM 이벤트를 지원하는 HTML 요소에 대한 정보가 포함되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-155">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers) &ndash; Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="3d10f-156">람다 식</span><span class="sxs-lookup"><span data-stu-id="3d10f-156">Lambda expressions</span></span>

<span data-ttu-id="3d10f-157">[람다 식](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)을 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-157">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="3d10f-158">요소 집합을 반복하는 경우와 같이 추가 값을 둘러싸는 것이 편리한 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-158">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="3d10f-159">다음 예제에서는 UI에서 선택할 경우 각각 이벤트 인수(`MouseEventArgs`)와 해당 단추 번호(`buttonNumber`)를 전달하여 `UpdateHeading`을 호출하는 세 개의 단추를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-159">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (`MouseEventArgs`) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="3d10f-160">람다 식의 `for` 루프에 루프 변수(`i`)를 직접 사용하면 **안 됩니다**.</span><span class="sxs-lookup"><span data-stu-id="3d10f-160">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="3d10f-161">직접 사용할 경우 모든 람다 식에서 동일한 변수가 사용되어 모든 람다의 `i` 값이 같아집니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-161">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="3d10f-162">항상 지역 변수(이전 예제의 `buttonNumber`)에 해당 값을 캡처하여 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-162">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="3d10f-163">EventCallback</span><span class="sxs-lookup"><span data-stu-id="3d10f-163">EventCallback</span></span>

<span data-ttu-id="3d10f-164">중첩된 구성 요소를 사용하는 일반적인 시나리오는 자식 구성 요소 이벤트가 발생할 때 부모 구성 요소의 메서드를 실행하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-164">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs.</span></span> <span data-ttu-id="3d10f-165">자식 구성 요소에서 발생하는 `onclick` 이벤트가 일반적인 사용 사례입니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-165">An `onclick` event occurring in the child component is a common use case.</span></span> <span data-ttu-id="3d10f-166">구성 요소 간에 이벤트를 공개하려면 `EventCallback`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-166">To expose events across components, use an `EventCallback`.</span></span> <span data-ttu-id="3d10f-167">부모 구성 요소는 자식 구성 요소의 `EventCallback`에 콜백 메서드를 할당할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-167">A parent component can assign a callback method to a child component's `EventCallback`.</span></span>

<span data-ttu-id="3d10f-168">샘플 앱(*Components/ChildComponent.razor*)의 `ChildComponent`는 단추의 `onclick` 처리기가 샘플의 `ParentComponent`에서 `EventCallback` 대리자를 수신하도록 설정된 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-168">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an `EventCallback` delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="3d10f-169">`EventCallback`은 주변 디바이스의 `onclick` 이벤트에 적합한 `MouseEventArgs` 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-169">The `EventCallback` is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="3d10f-170">`ParentComponent`는 자식의 `EventCallback<T>`(`OnClickCallback`)를 해당 `ShowMessage` 메서드에 설정합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-170">The `ParentComponent` sets the child's `EventCallback<T>` (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="3d10f-171">*Pages/ParentComponent.razor*:</span><span class="sxs-lookup"><span data-stu-id="3d10f-171">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@messageText</b></p>

@code {
    private string messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="3d10f-172">`ChildComponent`에서 단추를 선택한 경우</span><span class="sxs-lookup"><span data-stu-id="3d10f-172">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="3d10f-173">`ParentComponent`의 `ShowMessage` 메서드가 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-173">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="3d10f-174">`messageText`가 업데이트되고 `ParentComponent`에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-174">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="3d10f-175">[StateHasChanged](xref:blazor/lifecycle#state-changes) 호출은 콜백의 메서드(`ShowMessage`)에 필요하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-175">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="3d10f-176">자식 이벤트가 자식 내에서 실행되는 이벤트 처리기에서 다시 렌더링되는 구성 요소를 트리거하는 것처럼 `ParentComponent`를 다시 렌더링하기 위해 `StateHasChanged`가 자동으로 호출됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-176">`StateHasChanged` is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="3d10f-177">`EventCallback` 및 `EventCallback<T>`는 비동기 대리자를 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-177">`EventCallback` and `EventCallback<T>` permit asynchronous delegates.</span></span> <span data-ttu-id="3d10f-178">`EventCallback<T>`는 강력한 형식으로, 특정 인수 형식이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-178">`EventCallback<T>` is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="3d10f-179">`EventCallback`은 약한 형식으로, 모든 인수 형식을 허용합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-179">`EventCallback` is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />
```

<span data-ttu-id="3d10f-180">`InvokeAsync`를 사용하여 `EventCallback` 또는 `EventCallback<T>`를 호출하고 <xref:System.Threading.Tasks.Task>를 기다립니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-180">Invoke an `EventCallback` or `EventCallback<T>` with `InvokeAsync` and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="3d10f-181">이벤트 처리 및 바인딩 구성 요소 매개 변수에 `EventCallback` 및 `EventCallback<T>`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-181">Use `EventCallback` and `EventCallback<T>` for event handling and binding component parameters.</span></span>

<span data-ttu-id="3d10f-182">`EventCallback`보다 강력한 형식의 `EventCallback<T>`를 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-182">Prefer the strongly typed `EventCallback<T>` over `EventCallback`.</span></span> <span data-ttu-id="3d10f-183">`EventCallback<T>`는 구성 요소 사용자에게 더 나은 오류 피드백을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-183">`EventCallback<T>` provides better error feedback to users of the component.</span></span> <span data-ttu-id="3d10f-184">다른 UI 이벤트 처리기와 마찬가지로 이벤트 매개 변수 지정은 선택 사항입니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-184">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="3d10f-185">콜백에 전달되는 값이 없는 경우 `EventCallback`을 사용합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-185">Use `EventCallback` when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="3d10f-186">기본 작업 방지</span><span class="sxs-lookup"><span data-stu-id="3d10f-186">Prevent default actions</span></span>

<span data-ttu-id="3d10f-187">[`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) 지시문 특성을 사용하여 이벤트의 기본 작업을 방지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-187">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="3d10f-188">입력 디바이스에서 키를 선택하고 요소 포커스가 텍스트 상자에 놓이면 일반적으로 브라우저의 텍스트 상자에 키 문자가 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-188">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="3d10f-189">다음 예제에서는 `@onkeypress:preventDefault` 지시문 특성을 지정하여 기본 동작을 방지합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-189">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="3d10f-190">카운터가 증가하고 **+** 키가 `<input>` 요소의 값에 캡처되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-190">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            count++;
        }
    }
}
```

<span data-ttu-id="3d10f-191">값 없이 `@on{EVENT}:preventDefault` 특성을 지정하는 것은 `@on{EVENT}:preventDefault="true"`와 동일합니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-191">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="3d10f-192">특성 값으로 식을 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-192">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="3d10f-193">다음 예제에서 `shouldPreventDefault`는 `true` 또는 `false`로 설정되는 `bool` 필드입니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-193">In the following example, `shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="shouldPreventDefault" />
```

<span data-ttu-id="3d10f-194">기본 작업을 방지하기 위해 이벤트 처리기가 필요하지는 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-194">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="3d10f-195">이벤트 처리기와 기본 작업 시나리오를 독립적으로 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-195">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="3d10f-196">이벤트 전파 중지</span><span class="sxs-lookup"><span data-stu-id="3d10f-196">Stop event propagation</span></span>

<span data-ttu-id="3d10f-197">[`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) 지시문 특성을 사용하여 이벤트 전파를 중지할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-197">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="3d10f-198">다음 예제에서 확인란을 선택하면 두 번째 자식 `<div>`의 클릭 이벤트가 부모 `<div>`로 전파되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="3d10f-198">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
