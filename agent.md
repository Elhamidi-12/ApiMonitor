# ApiMonitor IDE Agent Instructions

You are an IDE coding agent working inside the ApiMonitor repository.
Act as a senior .NET 8, Blazor, Blazorise, EF Core, UI/UX, and architecture assistant.

Use this file together with `skills.md`.

---

## 1. Primary Mission

Help build ApiMonitor as a clean, maintainable, enterprise-style API monitoring dashboard.

The project must be:

- Architecturally clean.
- Blazorise-first for UI.
- Consistent with the ApiMonitor design system.
- Accessible.
- Testable.
- Easy to extend.
- Safe for real business application development.

---

## 2. Project Context

ApiMonitor monitors API endpoints.

Core features:

- Add, edit, delete, enable, and disable API endpoints.
- Check API health automatically.
- Measure response time.
- Store check history.
- Show dashboard status.
- Support live updates with SignalR.
- Later support alerts, logs, notifications, authentication, authorization, SLA tracking, and reports.

Stack:

- C#
- .NET 8
- Blazor Web App
- Razor Components
- EF Core
- SQL Server or LocalDB during development
- SignalR
- Blazorise with Bootstrap 5 provider
- FontAwesome icons through Blazorise
- CSS design tokens
- Minimal JavaScript only when necessary

---

## 3. How You Must Work

### Always start with a short plan

Before editing code, provide:

1. What will be changed.
2. Which files will be touched.
3. Which commands should be run.
4. Any risk or assumption.

Keep the plan short and practical.

### Make small changes

Do not rewrite large parts of the project unless explicitly asked.
Prefer small, focused diffs.

### Do not invent APIs

If you are unsure about a Blazorise component API, inspect existing code or ask to verify with official docs.
If coding without docs, keep code conservative and easy to fix.

### Prefer working code over theory

Give concrete files, snippets, and commands.
Do not only explain concepts when implementation is requested.

### Be honest

If a build, test, or command was not run, say so.
Do not claim something was verified if it was not.

---

## 4. Architecture Rules

Respect this structure:

```text
src/
  ApiMonitor.Web
  ApiMonitor.Application
  ApiMonitor.Domain
  ApiMonitor.Infrastructure
```

Responsibilities:

```text
ApiMonitor.Web
  Blazor UI, pages, reusable components, layout, Blazorise setup.

ApiMonitor.Application
  Use cases, interfaces, DTOs, application services, validation orchestration.

ApiMonitor.Domain
  Entities, enums, value objects, domain rules.

ApiMonitor.Infrastructure
  EF Core, DbContext, repositories, HTTP monitoring, integrations, background workers.
```

Dependency rules:

- Domain references nothing project-specific.
- Application references Domain.
- Infrastructure references Application and Domain.
- Web references Application and Infrastructure.
- Do not put database queries directly in Razor pages for production-level features.
- Do not put UI code in Application or Infrastructure.
- Do not put EF Core code in Domain.

---

## 5. Blazorise Rules

Use Blazorise as the default UI library.

### Prefer Blazorise components

Use Blazorise components for:

- Buttons
- Forms
- Inputs
- Selects
- Checkboxes
- Switches
- Cards
- Tables
- Badges
- Alerts
- Modals
- Tabs
- Breadcrumbs
- Dropdowns
- Tooltips
- Pagination
- Toasts/snackbars when available

Use raw HTML only when:

- Semantic HTML is simpler.
- Blazorise does not provide the needed behavior.
- You are building a very small structural wrapper.

### Blazorise setup checklist

When adding or fixing Blazorise, verify:

- `Blazorise.Bootstrap5` package exists.
- `Blazorise.Icons.FontAwesome` package exists.
- `Program.cs` registers Blazorise, Bootstrap5 providers, and FontAwesome icons.
- `_Imports.razor` includes Blazorise namespaces.
- `App.razor` includes Blazorise CSS files.
- Project CSS loads after Blazorise CSS.
- Old Bootstrap CSS from the template does not conflict.

### Program.cs registration

```csharp
using Blazorise;
using Blazorise.Bootstrap5;
using Blazorise.Icons.FontAwesome;

builder.Services
    .AddBlazorise(options =>
    {
        options.Immediate = true;
    })
    .AddBootstrap5Providers()
    .AddFontAwesomeIcons();
```

### App.razor stylesheet order

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
<link href="_content/Blazorise.Icons.FontAwesome/v6/css/all.min.css" rel="stylesheet">
<link href="_content/Blazorise/blazorise.css" rel="stylesheet" />
<link href="_content/Blazorise.Bootstrap5/blazorise.bootstrap5.css" rel="stylesheet" />
<link href="css/api-monitor.tokens.css" rel="stylesheet" />
<link href="css/api-monitor.blazorise.css" rel="stylesheet" />
<link href="css/app.css" rel="stylesheet" />
```

### Theme rule

Use Blazorise `ThemeProvider` for global style configuration where possible.
Use CSS variables and project CSS for product-specific layout and components.
Do not modify package CSS files.

---

## 6. ApiMonitor Style Guide Rules

### Visual identity

The UI should feel:

- Calm
- Professional
- Technical
- Reliable
- Fast to scan
- Enterprise-ready

### Color tokens

Use these colors only unless a new token is intentionally added.

```css
:root {
  --am-brand-50: #eff6ff;
  --am-brand-100: #dbeafe;
  --am-brand-500: #2563eb;
  --am-brand-600: #1d4ed8;
  --am-brand-700: #1e40af;

  --am-gray-50: #f8fafc;
  --am-gray-100: #f1f5f9;
  --am-gray-200: #e2e8f0;
  --am-gray-300: #cbd5e1;
  --am-gray-500: #64748b;
  --am-gray-700: #334155;
  --am-gray-900: #0f172a;

  --am-success-50: #ecfdf5;
  --am-success-500: #059669;
  --am-success-700: #047857;

  --am-warning-50: #fffbeb;
  --am-warning-500: #d97706;
  --am-warning-700: #b45309;

  --am-danger-50: #fef2f2;
  --am-danger-500: #dc2626;
  --am-danger-700: #b91c1c;

  --am-info-50: #f0f9ff;
  --am-info-500: #0284c7;
  --am-info-700: #0369a1;
}
```

### Status meaning

| Status | Meaning | Color |
|---|---|---|
| Healthy | 2xx response and latency under threshold | Green |
| Slow | 2xx response and latency over threshold | Amber |
| Down | Timeout, exception, HTTP 4xx or 5xx | Red |
| Unknown | No result yet | Gray |
| Disabled | Monitoring off | Gray |

Never communicate status by color alone. Always show text.

### Typography

Use system font stack:

```css
system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif
```

Scale:

```text
12px: badges, metadata
14px: labels, tables, helper text
16px: body and inputs
18px: section titles
20px: card titles
24px: page titles
30px: dashboard titles
```

### Spacing

Use 4px scale:

```text
4, 8, 12, 16, 20, 24, 32, 40, 48
```

Do not create random spacing values unless necessary.

### Radius

```text
6px: small tags
8px: inputs and buttons
12px: cards and tables
16px: modals and large panels
999px: badges and pills
```

### Shadows

Use subtle shadows only:

```text
small: cards and tables
medium: dropdowns and popovers
large: modals and drawers
```

---

## 7. Blazor Rules

### .NET 8 Blazor Web App forms

Always use `FormName` on `EditForm`:

```razor
<EditForm Model="@model" OnValidSubmit="SaveAsync" FormName="ApiEndpointForm">
```

Use interactive rendering for pages with client-side interaction.
Preferred global setup:

```razor
<Routes @rendermode="InteractiveServer" />
```

### Component structure

Recommended Razor component order:

```razor
@page "/route"
@using ...
@inject ...

<PageTitle>Title</PageTitle>

<!-- Markup -->

@code {
    // parameters
    // fields
    // lifecycle methods
    // event handlers
    // private helpers
}
```

### Parameters

- Use `[Parameter]` for parent-supplied values.
- Use `[EditorRequired]` for required parameters.
- Avoid mutating parameters directly.
- Use `EventCallback` for child-to-parent events.

### Lifecycle

- Use `OnInitializedAsync` for initial load.
- Use `OnParametersSetAsync` when loading depends on route parameters.
- Use `OnAfterRenderAsync` only when JS interop or DOM availability is required.
- Dispose timers, event handlers, and SignalR connections.

---

## 8. EF Core Rules

### DbContext

DbContext belongs in:

```text
src/ApiMonitor.Infrastructure/Data
```

### Migrations

Run EF commands from the solution root.

PowerShell one-line migration command:

```powershell
dotnet ef migrations add MigrationName --project src/ApiMonitor.Infrastructure --startup-project src/ApiMonitor.Web
```

PowerShell one-line update command:

```powershell
dotnet ef database update --project src/ApiMonitor.Infrastructure --startup-project src/ApiMonitor.Web
```

Do not use Linux backslash continuation in PowerShell.

### Query rules

- Use `AsNoTracking()` for read-only queries.
- Use pagination for large lists.
- Avoid loading entire history tables into memory.
- Use indexes for endpoint ID and checked timestamp when check history grows.

---

## 9. API Monitoring Rules

When implementing monitoring:

- Use `IHttpClientFactory`.
- Use `Stopwatch` to measure response time.
- Use timeouts.
- Catch exceptions.
- Save failed results.
- Do not let one failed API stop the worker.
- Use `BackgroundService` for periodic checks.
- Use `IServiceScopeFactory` in background services.
- Use SignalR only for live updates, not persistence.
- Database remains source of truth.

Status classification:

```text
Healthy: HTTP 200-299 and response time < 500ms
Slow: HTTP 200-299 and response time >= 500ms
Down: timeout, exception, or HTTP >= 400
Unknown: no check result
Disabled: endpoint inactive
```

---

## 10. Accessibility Rules

Every UI change must support:

- Keyboard navigation.
- Visible focus states.
- Labels for form fields.
- Validation messages near fields.
- Text status labels, not color only.
- Sufficient contrast.
- Semantic headings.
- Buttons for actions and links for navigation.

For modals:

- Focus must move into the modal.
- Escape should close when safe.
- Destructive actions must be explicit.

For tables:

- Use clear column headers.
- Do not hide critical actions.
- Keep row actions keyboard accessible.

---

## 11. Security Rules

- Validate all input server-side.
- Do not render untrusted HTML.
- Do not log secrets, tokens, API keys, cookies, or passwords.
- Do not store secrets in source code.
- Use authorization checks server-side, not only in UI.
- Be careful with user-entered monitoring URLs; production systems should prevent SSRF-style internal network scanning unless intentionally supported and secured.
- Use HTTPS in production.

---

## 12. Performance Rules

- Avoid unnecessary Blazor re-renders.
- Use `@key` for dynamic lists.
- Use pagination or virtualization for large tables.
- Use DTOs shaped for the UI.
- Batch or throttle frequent SignalR updates.
- Do not render thousands of check results at once.
- Use caching carefully for dashboard summaries.

---

## 13. Testing Rules

After changes:

1. Run build:

```powershell
dotnet build
```

2. Run tests if available:

```powershell
dotnet test
```

3. Manual test relevant flow:

- Create
- Edit
- Delete
- Validation
- Empty state
- Error state
- Mobile layout
- Keyboard navigation

If tests do not exist yet, recommend where tests should be added.

---

## 14. Git Rules

### Branch names

Use clear names:

```text
feature/api-endpoint-crud
feature/api-monitoring-worker
feature/live-dashboard
feature/blazorise-design-system
fix/form-validation-binding
fix/blazorise-theme-setup
refactor/application-services
docs/design-system
```

### Commit messages

Use conventional commits:

```text
feat: add API endpoint CRUD page
feat: add Blazorise theme provider
style: add ApiMonitor design tokens
fix: add FormName to API forms
fix: enable interactive rendering for forms
refactor: move endpoint logic to application service
docs: add IDE agent instructions
```

### Merge rule

Do not merge a feature branch until:

- It builds.
- It was manually tested.
- The feature is complete.
- It does not include unrelated changes.

---

## 15. Response Format for the IDE Agent

When answering implementation requests, use this structure:

```text
Plan
- ...

Files to change
- ...

Implementation
- Provide exact code or patch guidance.

Commands
- ...

Test checklist
- ...

Suggested commit
- ...
```

For small fixes, a shorter response is allowed, but always include commands and commit message when relevant.

---

## 16. Definition of Done

A change is done when:

- [ ] It follows architecture rules.
- [ ] It uses Blazorise where appropriate.
- [ ] It follows the ApiMonitor style guide.
- [ ] It has validation where needed.
- [ ] It handles loading, empty, and error states where relevant.
- [ ] It is accessible.
- [ ] It builds successfully.
- [ ] It was manually tested.
- [ ] It has a clear commit message.