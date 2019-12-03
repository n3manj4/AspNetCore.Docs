---
title: What's new in ASP.NET Core 3.1
author: rick-anderson
description: Learn about the new features in ASP.NET Core 3.1.
ms.author: riande
ms.custom: mvc
ms.date: 11/12/2019
no-loc: [Blazor, SignalR]
uid: aspnetcore-3.1
---
# What's new in ASP.NET Core 3.1

This article highlights the most significant changes in ASP.NET Core 3.1 with links to relevant documentation.

## Partial class support for Razor components

Razor components are now generated as partial classes. Code for a Razor component can be written using a code-behind file defined as a partial class rather than defining all the code for the component in a single file. For more information, see [Partial class support](xref:blazor/components#partial-class-support).

## Blazor Component Tag Helper and pass parameters to top-level components

In Blazor with ASP.NET Core 3.0, components were rendered into pages and views using an HTML Helper (`Html.RenderComponentAsync`). In ASP.NET Core 3.1, render a component from a page or view with the new Component Tag Helper:

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

The HTML Helper remains supported in ASP.NET Core 3.1, but the Component Tag Helper is recommended.

Blazor Server apps can now pass parameters to top-level components during the initial render. Previously you could only pass parameters to a top-level component with [RenderMode.Static](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static). With this release, both [RenderMode.Server](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server) and [RenderModel.ServerPrerendered](xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered) are supported. Any specified parameter values are serialized as JSON and included in the initial response.

For example, prerender a `Counter` component with an increment amount (`IncrementAmount`):

```razor
<component type="typeof(Counter)" render-mode="ServerPrerendered" 
    param-IncrementAmount="10" />
```

For more information, see [Integrate components into Razor Pages and MVC apps](xref:blazor/components#integrate-components-into-razor-pages-and-mvc-apps).

## Support for shared queues in HTTP.sys

[HTTP.sys](xref:fundamentals/servers/httpsys) supports creating anonymous request queues. In ASP.NET Core 3.1, we’ve added to ability to create or attach to an existing named HTTP.sys request queue. Creating or attaching to an existing named HTTP.sys request queue enables scenarios where the HTTP.Sys controller process that owns the queue is independent of the listener process. This independence makes it possible to preserve existing connections and enqueued requests between listener process restarts:

[!code-csharp[](sample/Program.cs?name=snippet)]

<!-- TODO
## Breaking changes for SameSite cookies
-->

## Prevent default actions for events in Blazor apps

Use the `@on{EVENT}:preventDefault` directive attribute to prevent the default action for an event. In the following example, the default action of displaying the key's character in the text box is prevented:

```razor
<input value="@_count" @onkeypress="KeyHandler" @onkeypress:preventDefault />
```

For more information, see [Prevent default actions](xref:blazor/components#prevent-default-actions).

## Stop event propagation in Blazor apps

Use the `@on{EVENT}:stopPropagation` directive attribute to stop event propagation. In the following example, selecting the check box prevents click events from the child `<div>` from propagating to the parent `<div>`:

```razor
<input @bind="_stopPropagation" type="checkbox" />

<div @onclick="OnSelectParentDiv">
    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="_stopPropagation">
        ...
    </div>
</div>

@code {
    private bool _stopPropagation = false;
}
```

For more information, see [Stop event propagation](xref:blazor/components#stop-event-propagation).

## Detailed errors during Blazor app development

When a Blazor app isn't functioning properly during development, receiving detailed error information from the app assists in troubleshooting and fixing the issue. When an error occurs, Blazor apps display a gold bar at the bottom of the screen:

* During development, the gold bar directs you to the browser console, where you can see the exception.
* In production, the gold bar notifies the user that an error has occurred and recommends refreshing the browser.

For more information, see [Detailed errors during development](xref:blazor/handle-errors#detailed-errors-during-development).