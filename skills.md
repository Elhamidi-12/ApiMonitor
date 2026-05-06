# ApiMonitor IDE Skills

This file defines reusable project skills for an IDE coding agent working on ApiMonitor.
Use it together with `agent.md`. This file tells the agent what repeatable workflows exist. `agent.md` tells the agent how to behave while executing them.

Project stack:

- C#
- .NET 8
- Blazor Web App
- Razor Components
- EF Core
- SignalR
- Blazorise with Bootstrap 5 provider
- CSS design tokens and project-specific styling
- Minimal JavaScript only when Blazor cannot solve the problem cleanly

Project goal:

ApiMonitor is an enterprise-style API monitoring dashboard. It allows users to register API endpoints, check their health, measure response time, store check history, show status dashboards, and later support alerts, logs, notifications, and SLA tracking.

---

## 1. Global Skill Rules

When working on this project, always follow these rules:

1. Respect the project architecture:
   - `ApiMonitor.Domain`: entities, enums, value objects, domain rules.
   - `ApiMonitor.Application`: interfaces, DTOs, use cases, services, validation orchestration.
   - `ApiMonitor.Infrastructure`: EF Core, repositories, HTTP checks, integrations, background workers.
   - `ApiMonitor.Web`: Blazor UI, Razor Components, layouts, pages, Blazorise theme setup.

2. Prefer Blazorise components for UI:
   - Use Blazorise components before raw HTML controls.
   - Use raw HTML only when Blazorise does not provide a suitable component or when semantic HTML is simpler.
   - Keep Bootstrap utility usage limited and intentional.
   - Customize Blazorise through `ThemeProvider`, CSS variables, and project-specific classes.

3. Use the project style guide:
   - Use ApiMonitor design tokens.
   - Use the status colors consistently.
   - Use spacing, typography, radius, and shadows from this file.
   - Do not introduce random colors, random spacing, or one-off styles.

4. Keep changes small:
   - One feature branch should implement one complete feature.
   - Do not mix unrelated refactors with feature work.
   - Prefer small, reviewable commits.

5. Validate work:
   - Run `dotnet build` after code changes.
   - Run tests when tests exist.
   - If EF Core model changes, create a migration.
   - If UI changes, check responsive, accessibility, loading, empty, and error states.

---

## 2. Skill: Architecture Guard

Use this skill whenever adding, moving, or refactoring code.

### Purpose

Protect the layered architecture and avoid messy dependencies.

### Dependency rules

Allowed references:

```text
ApiMonitor.Domain
  references nothing project-specific

ApiMonitor.Application
  references ApiMonitor.Domain

ApiMonitor.Infrastructure
  references ApiMonitor.Application
  references ApiMonitor.Domain

ApiMonitor.Web
  references ApiMonitor.Application
  references ApiMonitor.Infrastructure
  references ApiMonitor.Domain only when needed for simple UI models during early development
```

Preferred strict architecture over time:

```text
Web -> Application -> Domain
Infrastructure -> Application -> Domain
Domain -> nothing
```

### Rules

- Do not put EF Core code in Razor pages.
- Do not put HTTP monitoring logic in Razor pages.
- Do not put UI code in Application or Infrastructure.
- Do not put database attributes or EF Core dependencies into Domain unless intentionally accepted.
- Application should depend on abstractions, not infrastructure details.
- Infrastructure should implement Application interfaces.

### Checklist

Before finishing architecture-related work, verify:

- [ ] Domain has no UI dependency.
- [ ] Domain has no EF Core dependency.
- [ ] Application does not reference Web.
- [ ] Infrastructure does not reference Web.
- [ ] Razor pages call Application services, not DbContext directly.
- [ ] New services are registered in `Program.cs`.

---

## 3. Skill: Blazorise UI Implementation

Use this skill for any UI screen, form, dashboard card, table, modal, navigation, alert, or component.

### Purpose

Build consistent enterprise UI using Blazorise as the component foundation and ApiMonitor design tokens as the visual layer.

### Blazorise-first rule

Prefer these Blazorise components:

| UI need | Prefer Blazorise component |
|---|---|
| Page layout | Container, Row, Column, Layout components where useful |
| Buttons | Button |
| Links/navigation | NavLink, Breadcrumb, Menu where suitable |
| Text input | TextEdit |
| Number input | NumericEdit |
| Select/dropdown input | Select |
| Checkbox | Check |
| Toggle | Switch |
| Date input | DateEdit |
| Text area | MemoEdit |
| Cards | Card |
| Tables | Table or DataGrid if installed/approved |
| Badges/status | Badge |
| Alerts | Alert |
| Modal/dialog | Modal |
| Tabs | Tabs |
| Dropdown menu | Dropdown |
| Tooltip | Tooltip |
| Toast/snackbar | Snackbar/Toast if package and component are available |
| Pagination | Pagination |
| Loading | Progress, Spinner, Skeleton-like custom component if needed |

If exact Blazorise API names are uncertain, inspect existing project usage or the official Blazorise docs before coding.

### Setup skill

When adding Blazorise to the project, use Bootstrap 5 provider and FontAwesome icons:

```powershell
dotnet add src/ApiMonitor.Web package Blazorise.Bootstrap5
dotnet add src/ApiMonitor.Web package Blazorise.Icons.FontAwesome
```

In `Program.cs`:

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

In `_Imports.razor`:

```razor
@using Blazorise
@using Blazorise.Bootstrap5
@using Blazorise.Icons.FontAwesome
```

In .NET 8 Blazor Web App, add Blazorise CSS links in `Components/App.razor` inside the `head` section. Keep the project CSS after Blazorise CSS so project overrides win.

Recommended order:

```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
<link href="_content/Blazorise.Icons.FontAwesome/v6/css/all.min.css" rel="stylesheet">
<link href="_content/Blazorise/blazorise.css" rel="stylesheet" />
<link href="_content/Blazorise.Bootstrap5/blazorise.bootstrap5.css" rel="stylesheet" />
<link href="css/api-monitor.tokens.css" rel="stylesheet" />
<link href="css/api-monitor.blazorise.css" rel="stylesheet" />
<link href="css/app.css" rel="stylesheet" />
```

Important:

- Use the exact Blazorise CSS versions generated or recommended by installed packages when possible.
- If the default Blazor template includes older Bootstrap CSS, remove or replace it to avoid conflicts.

### ThemeProvider rule

Use `ThemeProvider` to centralize colors, typography, spacing, border radius, and component defaults.

Example root setup:

```razor
<ThemeProvider Theme="@ApiMonitorTheme">
    <Routes @rendermode="InteractiveServer" />
</ThemeProvider>

@code {
    private Theme ApiMonitorTheme = new()
    {
        ColorOptions = new ThemeColorOptions
        {
            Primary = "#2563EB",
            Secondary = "#64748B",
            Success = "#059669",
            Warning = "#D97706",
            Danger = "#DC2626",
            Info = "#0284C7",
            Light = "#F8FAFC",
            Dark = "#0F172A",
            Link = "#2563EB"
        },
        TextColorOptions = new ThemeTextColorOptions
        {
            Body = "#0F172A",
            Muted = "#64748B",
            Primary = "#1D4ED8",
            Success = "#047857",
            Warning = "#B45309",
            Danger = "#B91C1C",
            Info = "#0369A1"
        },
        BackgroundOptions = new ThemeBackgroundOptions
        {
            Body = "#F8FAFC",
            Light = "#F8FAFC",
            Primary = "#2563EB",
            Success = "#ECFDF5",
            Warning = "#FFFBEB",
            Danger = "#FEF2F2",
            Info = "#F0F9FF"
        },
        FontOptions = new ThemeFontOptions
        {
            Family = "system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif",
            Size = "16px",
            Weight = "400"
        },
        ButtonOptions = new ThemeButtonOptions
        {
            BorderRadius = "0.625rem",
            Padding = "0.625rem 1rem"
        },
        InputOptions = new ThemeInputOptions
        {
            BorderRadius = "0.625rem"
        }
    };
}
```

If a property name does not compile with the installed Blazorise version, inspect the installed package API and adjust while preserving the same design intent.

---

## 4. Skill: ApiMonitor Style Guide

Use this skill whenever building UI or CSS.

### Brand personality

ApiMonitor should feel:

- Professional
- Calm
- Technical
- Reliable
- Fast to scan
- Enterprise-ready

### Color palette

#### Brand colors

| Token | Hex | Use |
|---|---|---|
| `--am-brand-50` | `#EFF6FF` | Light brand backgrounds |
| `--am-brand-100` | `#DBEAFE` | Focus rings, subtle hover |
| `--am-brand-500` | `#2563EB` | Primary buttons, links |
| `--am-brand-600` | `#1D4ED8` | Primary hover, active nav |
| `--am-brand-700` | `#1E40AF` | Strong brand emphasis |

#### Neutral colors

| Token | Hex | Use |
|---|---|---|
| `--am-gray-50` | `#F8FAFC` | App background |
| `--am-gray-100` | `#F1F5F9` | Muted panels |
| `--am-gray-200` | `#E2E8F0` | Borders |
| `--am-gray-300` | `#CBD5E1` | Strong borders |
| `--am-gray-500` | `#64748B` | Muted text |
| `--am-gray-700` | `#334155` | Secondary text |
| `--am-gray-900` | `#0F172A` | Main text |

#### Status colors

| Status | Main | Soft background | Strong text | Meaning |
|---|---|---|---|---|
| Healthy | `#059669` | `#ECFDF5` | `#047857` | API is working normally |
| Slow | `#D97706` | `#FFFBEB` | `#B45309` | API works but latency is high |
| Down | `#DC2626` | `#FEF2F2` | `#B91C1C` | API failed, timed out, or returned error |
| Info | `#0284C7` | `#F0F9FF` | `#0369A1` | Neutral information |
| Unknown | `#64748B` | `#F1F5F9` | `#475569` | No check result yet |
| Disabled | `#64748B` | `#F1F5F9` | `#475569` | Monitoring is turned off |

### CSS token file

Create or maintain:

```text
src/ApiMonitor.Web/wwwroot/css/api-monitor.tokens.css
```

Content:

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

  --am-bg: var(--am-gray-50);
  --am-surface: #ffffff;
  --am-surface-muted: var(--am-gray-100);
  --am-border: var(--am-gray-200);
  --am-border-strong: var(--am-gray-300);
  --am-text: var(--am-gray-900);
  --am-text-muted: var(--am-gray-500);

  --am-font-family: system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --am-font-size-xs: 0.75rem;
  --am-font-size-sm: 0.875rem;
  --am-font-size-md: 1rem;
  --am-font-size-lg: 1.125rem;
  --am-font-size-xl: 1.25rem;
  --am-font-size-2xl: 1.5rem;
  --am-font-size-3xl: 1.875rem;

  --am-space-1: 0.25rem;
  --am-space-2: 0.5rem;
  --am-space-3: 0.75rem;
  --am-space-4: 1rem;
  --am-space-5: 1.25rem;
  --am-space-6: 1.5rem;
  --am-space-8: 2rem;
  --am-space-10: 2.5rem;
  --am-space-12: 3rem;

  --am-radius-sm: 0.375rem;
  --am-radius-md: 0.5rem;
  --am-radius-lg: 0.75rem;
  --am-radius-xl: 1rem;
  --am-radius-full: 999px;

  --am-shadow-sm: 0 1px 2px rgba(15, 23, 42, 0.06);
  --am-shadow-md: 0 8px 20px rgba(15, 23, 42, 0.08);
  --am-shadow-lg: 0 16px 40px rgba(15, 23, 42, 0.12);

  --am-sidebar-width: 260px;
  --am-topbar-height: 64px;
  --am-content-max-width: 1440px;
}
```

### Typography

Use system fonts unless the product later defines a brand font.

| Token | Size | Use |
|---|---:|---|
| `xs` | 12px | Badges, helper text, metadata |
| `sm` | 14px | Table text, labels, secondary text |
| `md` | 16px | Body text, form controls |
| `lg` | 18px | Section headings |
| `xl` | 20px | Card titles |
| `2xl` | 24px | Page titles |
| `3xl` | 30px | Dashboard hero titles |

Rules:

- Page title: 24-30px, weight 700.
- Card title: 18-20px, weight 700.
- Labels: 14px, weight 600-700.
- Body: 16px.
- Table text: 14px.

### Spacing

Use a 4px-based scale:

| Token | Value | Use |
|---|---:|---|
| `space-1` | 4px | Tight gaps, icons |
| `space-2` | 8px | Small gaps |
| `space-3` | 12px | Form field internal spacing |
| `space-4` | 16px | Default component spacing |
| `space-5` | 20px | Card padding |
| `space-6` | 24px | Page section gaps |
| `space-8` | 32px | Large sections |
| `space-10` | 40px | Major layout gaps |
| `space-12` | 48px | Hero/page top spacing |

### Radius

| Token | Value | Use |
|---|---:|---|
| `sm` | 6px | Small tags, compact controls |
| `md` | 8px | Inputs, buttons |
| `lg` | 12px | Cards, tables |
| `xl` | 16px | Modals, large panels |
| `full` | 999px | Badges, pills, switches |

### Shadows

Use shadows sparingly.

- `shadow-sm`: cards and table wrappers.
- `shadow-md`: dropdowns, popovers, floating panels.
- `shadow-lg`: modals and drawers.

### Breakpoints

| Name | Width | Behavior |
|---|---:|---|
| Mobile | `< 640px` | Single column, full-width actions |
| Tablet | `640px - 1100px` | Two-column dashboard cards |
| Desktop | `> 1100px` | Sidebar and multi-column dashboard |

### Status rules

| Status | Condition |
|---|---|
| Healthy | HTTP 200-299 and response time under 500ms |
| Slow | HTTP 200-299 and response time 500ms or higher |
| Down | Timeout, exception, HTTP 400 or higher |
| Unknown | No check result yet |
| Disabled | Endpoint is inactive |

Status priority for sorting:

```text
Down -> Slow -> Unknown -> Disabled -> Healthy
```

---

## 5. Skill: Blazorise Styling Overrides

Use this skill when customizing Blazorise components.

### Preferred CSS file

Create or maintain:

```text
src/ApiMonitor.Web/wwwroot/css/api-monitor.blazorise.css
```

### Rules

- Do not edit Blazorise package CSS.
- Do not override random generated selectors unless unavoidable.
- Prefer project wrapper classes and Blazorise theme options.
- Keep selectors low-specificity.
- Use tokens from `api-monitor.tokens.css`.

### Example overrides

```css
.am-page {
  max-width: var(--am-content-max-width);
  margin: 0 auto;
  padding: var(--am-space-6);
}

.am-page-header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: var(--am-space-4);
  margin-bottom: var(--am-space-6);
}

.am-card-surface {
  background: var(--am-surface);
  border: 1px solid var(--am-border);
  border-radius: var(--am-radius-lg);
  box-shadow: var(--am-shadow-sm);
}

.am-status-badge {
  display: inline-flex;
  align-items: center;
  gap: var(--am-space-1);
  border-radius: var(--am-radius-full);
  padding: 0.25rem 0.625rem;
  font-size: var(--am-font-size-xs);
  font-weight: 700;
}

.am-status-healthy {
  background: var(--am-success-50);
  color: var(--am-success-700);
}

.am-status-slow {
  background: var(--am-warning-50);
  color: var(--am-warning-700);
}

.am-status-down {
  background: var(--am-danger-50);
  color: var(--am-danger-700);
}

.am-status-unknown,
.am-status-disabled {
  background: var(--am-gray-100);
  color: var(--am-gray-500);
}
```

---

## 6. Skill: API Endpoint CRUD

Use this skill for the API endpoint management feature.

### Feature scope

- Create API endpoint.
- List API endpoints.
- Edit API endpoint.
- Delete API endpoint.
- Enable or disable monitoring.
- Validate name, URL, method, interval.

### Entity

```csharp
public sealed class ApiEndpoint
{
    public Guid Id { get; set; } = Guid.NewGuid();
    public string Name { get; set; } = string.Empty;
    public string Url { get; set; } = string.Empty;
    public string Method { get; set; } = "GET";
    public bool IsActive { get; set; } = true;
    public int CheckIntervalSeconds { get; set; } = 10;
    public DateTime CreatedAtUtc { get; set; } = DateTime.UtcNow;
}
```

### UI requirements

- Use Blazorise form components.
- Use visible labels.
- Use validation messages.
- Use `FormName` on every `EditForm` in .NET 8 Blazor Web App.
- Use an empty state when no APIs exist.
- Do not show raw `True` or `False`; show `Active` or `Disabled`.
- Delete requires confirmation.

### Recommended routes

```text
/apis
/apis/create
/apis/edit/{id:guid}
/apis/{id:guid}
```

---

## 7. Skill: Monitoring Worker

Use this skill when building the background API monitoring engine.

### Feature scope

- Load active endpoints.
- Call endpoint URLs with `HttpClient`.
- Measure response time with `Stopwatch`.
- Classify result as Healthy, Slow, Down, Unknown, or Disabled.
- Save `ApiCheckResult` to database.
- Publish live update through SignalR later.

### Rules

- Use `BackgroundService` for periodic checks.
- Use `IServiceScopeFactory` inside background workers to create scoped services.
- Use `IHttpClientFactory` for HTTP clients.
- Set timeouts.
- Catch exceptions and save failed results.
- Do not allow one endpoint failure to stop the worker.
- Limit concurrency when many endpoints exist.

### Result entity

```csharp
public sealed class ApiCheckResult
{
    public Guid Id { get; set; } = Guid.NewGuid();
    public Guid ApiEndpointId { get; set; }
    public int? StatusCode { get; set; }
    public long ResponseTimeMs { get; set; }
    public bool IsSuccess { get; set; }
    public string? ErrorMessage { get; set; }
    public DateTime CheckedAtUtc { get; set; } = DateTime.UtcNow;
}
```

---

## 8. Skill: SignalR Live Dashboard

Use this skill when adding real-time dashboard behavior.

### Feature scope

- Create dashboard hub.
- Background worker publishes check result updates.
- Blazor dashboard subscribes to updates.
- UI updates latest status without full page refresh.

### Rules

- SignalR is for live updates, not source of truth.
- Database remains the source of truth.
- Use DTOs for messages.
- Unsubscribe/dispose hub connections when component is disposed.
- Avoid high-frequency UI spam; batch updates if needed.

---

## 9. Skill: EF Core Database

Use this skill when changing persistence.

### Rules

- DbContext lives in `ApiMonitor.Infrastructure/Data`.
- Migrations should be created with Infrastructure as project and Web as startup project.
- Use PowerShell one-line commands on Windows unless using backtick continuation.

### Commands

From solution root:

```powershell
dotnet ef migrations add MigrationName --project src/ApiMonitor.Infrastructure --startup-project src/ApiMonitor.Web
```

```powershell
dotnet ef database update --project src/ApiMonitor.Infrastructure --startup-project src/ApiMonitor.Web
```

### Common mistake

Do not run commands from the wrong folder. The solution root should contain:

```text
ApiMonitor.sln
src/
```

---

## 10. Skill: Validation and Forms

Use this skill for all forms.

### Rules

- Validate on submit.
- Show field-level messages.
- Show validation summary for complex forms.
- Use friendly messages.
- Do not rely on database exceptions for validation.
- Use `FormName` with `EditForm`.
- Use interactive render mode for interactive Blazor forms.

### Example validation messages

| Field | Message |
|---|---|
| Name | API name is required. |
| URL | Enter a valid URL, for example https://api.example.com/health. |
| Method | Choose a supported HTTP method. |
| CheckIntervalSeconds | Check interval must be between 5 and 300 seconds. |

---

## 11. Skill: Testing and QA

Use this skill before merge.

### Minimum checks

- `dotnet build`
- Manual test the changed workflow.
- Verify validation.
- Verify empty/loading/error states when applicable.
- Verify mobile width around 360px.
- Verify keyboard navigation for forms and actions.

### Future test tools

- xUnit for unit tests.
- bUnit for Blazor component tests.
- Playwright for end-to-end UI tests.
- Testcontainers for database integration tests.

---

## 12. Skill: Git Workflow

Use this skill whenever creating branches or commits.

### Branch names

```text
feature/api-endpoint-crud
feature/api-monitoring-worker
feature/live-dashboard
feature/blazorise-design-system
fix/form-validation-binding
fix/ef-migration-path
refactor/application-services
docs/design-system
```

### Commit message format

Use conventional style:

```text
feat: add API endpoint list page
feat: add create API endpoint form
fix: enable interactive render mode for forms
style: add ApiMonitor design tokens
refactor: move API endpoint logic to application service
docs: add Blazorise design system guide
```

### Merge rule

Merge a branch only when the feature is complete, builds successfully, and has been manually tested.

---

## 13. Skill: Troubleshooting

Use this skill for common project issues.

### PowerShell multiline EF error

If PowerShell shows `Missing expression after unary operator '--'`, the command was split incorrectly.
Use one line:

```powershell
dotnet ef migrations add InitialCreate --project src/ApiMonitor.Infrastructure --startup-project src/ApiMonitor.Web
```

### Wrong folder EF error

If EF says it cannot retrieve project metadata, verify that the command is running from solution root:

```powershell
dir
```

Expected:

```text
ApiMonitor.sln
src
```

### Blazor form values not binding

For .NET 8 Blazor Web App:

- Add `FormName` to `EditForm`.
- Enable interactive render mode globally or page-level.
- Prefer global setup:

```razor
<Routes @rendermode="InteractiveServer" />
```

### Blazorise components not styled

Check:

- Packages installed.
- Blazorise services registered in `Program.cs`.
- Blazorise CSS links added in `App.razor`.
- Project CSS loads after Blazorise CSS.
- Old Bootstrap references are removed or updated.

---

## 14. Definition of Done

A feature is done only when:

- [ ] It follows the architecture rules.
- [ ] It uses Blazorise for UI where appropriate.
- [ ] It follows the ApiMonitor style guide.
- [ ] It has validation, loading, empty, and error states where relevant.
- [ ] It is accessible by keyboard.
- [ ] It builds successfully.
- [ ] It has been manually tested.
- [ ] It has a clear commit message.