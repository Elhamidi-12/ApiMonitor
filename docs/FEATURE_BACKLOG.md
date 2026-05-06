# ApiMonitor Full Feature Roadmap & Task Backlog

**Project:** ApiMonitor  
**Stack:** C#, .NET 8, Blazor Web App, Razor Components, EF Core, SignalR, Blazorise, Bootstrap 5, FontAwesome  
**Architecture:** Clean/layered architecture with `Web`, `Application`, `Domain`, and `Infrastructure` projects  
**Purpose:** This document defines the full feature plan from 0% to 100%, including tasks, branches, acceptance criteria, and suggested commit messages.

---

## How to use this backlog

Use this document as your project roadmap.

Recommended workflow:

```text
1 feature = 1 branch
1 task group = 1 or more commits
Merge only when the feature works and builds successfully
```

Branch format:

```text
feature/<feature-name>
fix/<bug-name>
refactor/<area-name>
docs/<document-name>
test/<test-area>
```

Commit format:

```text
feat: add ...
fix: correct ...
refactor: improve ...
docs: add ...
test: add ...
style: apply ...
chore: configure ...
```

---

# Completion Overview

| Completion | Phase | Goal |
|---:|---|---|
| 0% - 5% | Project foundation | Create solution, projects, references, Git workflow |
| 5% - 10% | Design system & UI foundation | Add Blazorise, theme, layout, shared UI components |
| 10% - 20% | API endpoint management | CRUD for APIs |
| 20% - 35% | Monitoring engine | Background checks, HTTP calls, response time, results |
| 35% - 45% | Dashboard & real-time updates | Live dashboard with SignalR |
| 45% - 55% | History, charts, logs | Check history, charts, filtering, logs |
| 55% - 65% | Alerts | Alert rules, active alerts, alert evaluation |
| 65% - 72% | Auth & roles | Login, roles, permission-based UI |
| 72% - 80% | Environments, groups, settings | Dev/staging/prod, service groups, app settings |
| 80% - 88% | Security & reliability | Validation, SSRF protection, retry, timeout, audit |
| 88% - 94% | Testing & quality | Unit, component, integration, accessibility, E2E |
| 94% - 100% | Production polish & deployment | Docs, Docker, CI/CD, release, final UX polish |

---

# 0% - 5%: Project Foundation

## EPIC AM-000: Solution Setup

**Goal:** Create the base solution using clean/layered architecture.

**Branch:**

```bash
feature/project-foundation
```

## AM-001: Create solution and projects

### Tasks

- [ ] Create solution `ApiMonitor.sln`
- [ ] Create `src/ApiMonitor.Web`
- [ ] Create `src/ApiMonitor.Domain`
- [ ] Create `src/ApiMonitor.Application`
- [ ] Create `src/ApiMonitor.Infrastructure`
- [ ] Add all projects to solution
- [ ] Add project references:
  - [ ] `Application -> Domain`
  - [ ] `Infrastructure -> Application`
  - [ ] `Infrastructure -> Domain`
  - [ ] `Web -> Application`
  - [ ] `Web -> Infrastructure`
  - [ ] `Web -> Domain`
- [ ] Build solution successfully

### Acceptance criteria

- [ ] `dotnet build` succeeds
- [ ] Solution opens in Visual Studio
- [ ] Blazor app runs
- [ ] Project references are correct

### Commit

```bash
git commit -m "chore: initialize solution structure"
```

---

## AM-002: Add repository files

### Tasks

- [ ] Add `.gitignore`
- [ ] Add `README.md`
- [ ] Add `docs/` folder
- [ ] Add `docs/FEATURE_BACKLOG.md`
- [ ] Add `docs/DESIGN_SYSTEM.html`
- [ ] Add `agent.md`
- [ ] Add `skills.md`
- [ ] Add setup instructions to README
- [ ] Add run instructions to README

### Acceptance criteria

- [ ] New developer can clone and run project from README
- [ ] Documentation files are in repo

### Commit

```bash
git commit -m "docs: add project documentation foundation"
```

---

## AM-003: Configure app settings

### Tasks

- [ ] Add `appsettings.Development.json`
- [ ] Add connection string placeholder
- [ ] Add monitoring settings section
- [ ] Add logging settings section
- [ ] Add environment-specific configuration pattern

### Example configuration

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ApiMonitorDb;Trusted_Connection=True;"
  },
  "Monitoring": {
    "DefaultTimeoutSeconds": 10,
    "DefaultCheckIntervalSeconds": 30,
    "SlowThresholdMilliseconds": 500,
    "MaxConcurrentChecks": 10
  }
}
```

### Acceptance criteria

- [ ] App reads configuration successfully
- [ ] No secrets are committed

### Commit

```bash
git commit -m "chore: configure application settings"
```

---

# 5% - 10%: Design System & UI Foundation

## EPIC AM-010: Blazorise Setup

**Goal:** Use Blazorise as the UI component framework and customize it with the ApiMonitor style guide.

**Branch:**

```bash
feature/blazorise-ui-foundation
```

---

## AM-011: Install Blazorise packages

### Tasks

- [ ] Install Blazorise core package
- [ ] Install Bootstrap 5 provider
- [ ] Install FontAwesome icon package
- [ ] Add Blazorise CSS references
- [ ] Add Bootstrap CSS reference
- [ ] Add FontAwesome CSS reference
- [ ] Register Blazorise services in `Program.cs`
- [ ] Verify UI components render correctly

### Typical packages

```bash
dotnet add src/ApiMonitor.Web package Blazorise
dotnet add src/ApiMonitor.Web package Blazorise.Bootstrap5
dotnet add src/ApiMonitor.Web package Blazorise.Icons.FontAwesome
```

### Acceptance criteria

- [ ] Blazorise components render
- [ ] No console errors
- [ ] App builds and runs

### Commit

```bash
git commit -m "chore: add Blazorise UI framework"
```

---

## AM-012: Add ApiMonitor theme tokens

### Tasks

- [ ] Create `wwwroot/css/design-tokens.css`
- [ ] Add color palette
- [ ] Add typography scale
- [ ] Add spacing scale
- [ ] Add radius tokens
- [ ] Add shadow tokens
- [ ] Add status colors:
  - [ ] Healthy
  - [ ] Slow
  - [ ] Down
  - [ ] Unknown
  - [ ] Disabled
- [ ] Add dark mode tokens
- [ ] Import tokens into `app.css`

### Acceptance criteria

- [ ] CSS tokens are available globally
- [ ] Tokens match design system
- [ ] No hard-coded random colors in pages

### Commit

```bash
git commit -m "style: add ApiMonitor design tokens"
```

---

## AM-013: Configure Blazorise theme

### Tasks

- [ ] Add `ThemeProvider`
- [ ] Configure primary, secondary, success, warning, danger colors
- [ ] Configure typography
- [ ] Configure border radius
- [ ] Configure button styles
- [ ] Configure form input styles
- [ ] Test light theme
- [ ] Prepare dark theme strategy

### Acceptance criteria

- [ ] Blazorise components use ApiMonitor colors
- [ ] Buttons, inputs, cards, alerts visually match style guide
- [ ] Theme configuration is centralized

### Commit

```bash
git commit -m "style: configure Blazorise theme"
```

---

## AM-014: Create application shell layout

### Tasks

- [ ] Create/modify `MainLayout.razor`
- [ ] Add sidebar navigation
- [ ] Add topbar
- [ ] Add main content area
- [ ] Add responsive mobile navigation
- [ ] Add active navigation state
- [ ] Add app logo/name
- [ ] Add placeholder user menu
- [ ] Add theme toggle placeholder

### Navigation items

```text
Dashboard
APIs
Logs
Alerts
Settings
```

### Acceptance criteria

- [ ] Layout works on desktop
- [ ] Layout works on mobile
- [ ] Current route is visible in nav
- [ ] Main content does not overlap sidebar/topbar

### Commit

```bash
git commit -m "feat: add application shell layout"
```

---

## AM-015: Create shared UI components

### Tasks

Create reusable components:

- [ ] `PageHeader.razor`
- [ ] `StatusBadge.razor`
- [ ] `MetricCard.razor`
- [ ] `EmptyState.razor`
- [ ] `LoadingState.razor`
- [ ] `ErrorState.razor`
- [ ] `ConfirmDialog.razor`
- [ ] `FormActions.razor`
- [ ] `SearchBox.razor`
- [ ] `FilterBar.razor`

### Acceptance criteria

- [ ] Components are reusable
- [ ] Components use Blazorise where appropriate
- [ ] Components use ApiMonitor tokens
- [ ] Components have clear parameters

### Commit

```bash
git commit -m "feat: add shared UI components"
```

---

# 10% - 20%: API Endpoint Management

## EPIC AM-020: API Endpoint CRUD

**Goal:** Users can add, view, edit, delete, enable, and disable monitored APIs.

**Branch:**

```bash
feature/api-endpoint-crud
```

---

## AM-021: Add domain entities

### Tasks

- [ ] Create `ApiEndpoint`
- [ ] Create `ApiCheckResult`
- [ ] Add basic validation properties
- [ ] Add timestamps
- [ ] Add `IsActive`
- [ ] Add `CheckIntervalSeconds`
- [ ] Add HTTP method field
- [ ] Add navigation property from check result to endpoint

### Acceptance criteria

- [ ] Domain project builds
- [ ] Entities are clean and independent from EF-specific logic where possible

### Commit

```bash
git commit -m "feat: add API endpoint domain entities"
```

---

## AM-022: Add EF Core database

### Tasks

- [ ] Install EF Core packages
- [ ] Create `ApiMonitorDbContext`
- [ ] Add `DbSet<ApiEndpoint>`
- [ ] Add `DbSet<ApiCheckResult>`
- [ ] Configure entity rules in `OnModelCreating`
- [ ] Add connection string
- [ ] Register DbContext in `Program.cs`
- [ ] Create initial migration
- [ ] Update database

### Acceptance criteria

- [ ] Database is created
- [ ] Tables are created
- [ ] App builds and runs

### Commit

```bash
git commit -m "feat: configure EF Core database"
```

---

## AM-023: Add API endpoint service

### Tasks

- [ ] Create `IApiEndpointService`
- [ ] Add `GetAllAsync`
- [ ] Add `GetByIdAsync`
- [ ] Add `CreateAsync`
- [ ] Add `UpdateAsync`
- [ ] Add `DeleteAsync`
- [ ] Register service in DI
- [ ] Add null handling
- [ ] Add validation before save

### Acceptance criteria

- [ ] Service can create, read, update, delete endpoints
- [ ] Service is injected into Blazor pages
- [ ] No database logic exists directly inside Razor pages

### Commit

```bash
git commit -m "feat: add API endpoint service"
```

---

## AM-024: Add API list page

### Route

```text
/apis
```

### Tasks

- [ ] Create `Apis.razor`
- [ ] Load endpoints from service
- [ ] Display table/list
- [ ] Show empty state when no APIs exist
- [ ] Add loading state
- [ ] Add edit button
- [ ] Add create button
- [ ] Show Active/Disabled instead of True/False
- [ ] Add search by name/URL
- [ ] Add filter by active/disabled

### Acceptance criteria

- [ ] Page lists APIs from database
- [ ] Empty state works
- [ ] Search works
- [ ] Filter works
- [ ] UI follows design system

### Commit

```bash
git commit -m "feat: add API endpoints list page"
```

---

## AM-025: Add create API page

### Route

```text
/apis/create
```

### Tasks

- [ ] Create form page
- [ ] Add Blazorise text fields
- [ ] Add name field
- [ ] Add URL field
- [ ] Add method select
- [ ] Add check interval field
- [ ] Add active checkbox/toggle
- [ ] Add validation
- [ ] Add `FormName`
- [ ] Add save action
- [ ] Add cancel action
- [ ] Redirect after save
- [ ] Show success toast

### Acceptance criteria

- [ ] Valid endpoint can be created
- [ ] Empty name is rejected
- [ ] Invalid URL is rejected
- [ ] Check interval range is validated
- [ ] Page redirects to `/apis`

### Commit

```bash
git commit -m "feat: add create API endpoint page"
```

---

## AM-026: Add edit API page

### Route

```text
/apis/edit/{id}
```

### Tasks

- [ ] Load endpoint by ID
- [ ] Show loading state
- [ ] Show not found state
- [ ] Edit name
- [ ] Edit URL
- [ ] Edit method
- [ ] Edit interval
- [ ] Toggle active/disabled
- [ ] Save changes
- [ ] Cancel back to list
- [ ] Show success toast

### Acceptance criteria

- [ ] Existing endpoint can be edited
- [ ] Invalid edits are rejected
- [ ] Not found ID shows user-friendly error
- [ ] Save redirects or updates cleanly

### Commit

```bash
git commit -m "feat: add edit API endpoint page"
```

---

## AM-027: Add delete API endpoint

### Tasks

- [ ] Add delete button
- [ ] Add confirmation dialog
- [ ] Prevent accidental submit inside forms
- [ ] Delete endpoint
- [ ] Delete related check results or configure cascade
- [ ] Redirect after delete
- [ ] Show success toast

### Acceptance criteria

- [ ] Delete requires confirmation
- [ ] Delete removes endpoint
- [ ] List updates after delete
- [ ] No accidental delete from wrong button type

### Commit

```bash
git commit -m "feat: add delete API endpoint functionality"
```

---

## AM-028: Improve API endpoint validation

### Tasks

- [ ] Add Data Annotations or FluentValidation
- [ ] Add custom messages
- [ ] Validate supported HTTP methods
- [ ] Validate URL format
- [ ] Validate interval range
- [ ] Prevent duplicate endpoint names if required
- [ ] Prevent duplicate URLs if required

### Acceptance criteria

- [ ] Validation messages are friendly
- [ ] Invalid data cannot be saved
- [ ] Existing bad rows can be corrected or removed

### Commit

```bash
git commit -m "feat: improve API endpoint validation"
```

---

# 20% - 35%: Monitoring Engine

## EPIC AM-030: API Monitoring Worker

**Goal:** Automatically check APIs, measure response time, classify status, and store results.

**Branch:**

```bash
feature/api-monitoring-worker
```

---

## AM-031: Add monitoring domain concepts

### Tasks

- [ ] Add enum `ApiHealthStatus`
  - [ ] Healthy
  - [ ] Slow
  - [ ] Down
  - [ ] Unknown
  - [ ] Disabled
- [ ] Add status field to result or create computed status model
- [ ] Add slow threshold setting
- [ ] Add timeout setting
- [ ] Add status classification rules

### Status rules

```text
Disabled = endpoint IsActive false
Unknown = no check result yet
Healthy = 2xx and response time < slow threshold
Slow = 2xx and response time >= slow threshold
Down = timeout, exception, 4xx, 5xx
```

### Acceptance criteria

- [ ] Status rules are centralized
- [ ] UI and backend use same status meaning

### Commit

```bash
git commit -m "feat: add API health status model"
```

---

## AM-032: Add API health checker abstraction

### Tasks

- [ ] Create `IApiHealthChecker`
- [ ] Create request model for check input
- [ ] Create result model for check output
- [ ] Include:
  - [ ] Status code
  - [ ] Response time
  - [ ] Error message
  - [ ] Checked time
  - [ ] Success flag
- [ ] Add cancellation token support

### Acceptance criteria

- [ ] Application can call checker without knowing HTTP implementation

### Commit

```bash
git commit -m "feat: add API health checker abstraction"
```

---

## AM-033: Implement HTTP API health checker

### Tasks

- [ ] Use `IHttpClientFactory`
- [ ] Use `Stopwatch`
- [ ] Send HTTP request
- [ ] Support GET first
- [ ] Handle timeout
- [ ] Handle DNS errors
- [ ] Handle SSL errors
- [ ] Handle 4xx/5xx
- [ ] Store error message safely
- [ ] Set User-Agent header if needed
- [ ] Add timeout from configuration

### Acceptance criteria

- [ ] Valid API returns success
- [ ] Broken URL returns Down result
- [ ] Slow API records long response time
- [ ] 500 API records failed status
- [ ] Exceptions do not crash app

### Commit

```bash
git commit -m "feat: implement HTTP API health checker"
```

---

## AM-034: Add check result persistence

### Tasks

- [ ] Create `IApiCheckResultService`
- [ ] Add method to save result
- [ ] Add method to get latest result per endpoint
- [ ] Add method to get history for endpoint
- [ ] Add EF Core queries
- [ ] Add indexes:
  - [ ] `ApiEndpointId`
  - [ ] `CheckedAtUtc`
  - [ ] `ApiEndpointId + CheckedAtUtc`
- [ ] Add migration

### Acceptance criteria

- [ ] Check results are saved
- [ ] Latest result can be queried efficiently
- [ ] History can be queried by endpoint

### Commit

```bash
git commit -m "feat: persist API check results"
```

---

## AM-035: Add background monitoring worker

### Tasks

- [ ] Create `ApiMonitoringWorker : BackgroundService`
- [ ] Use `IServiceScopeFactory`
- [ ] Load active endpoints
- [ ] Check endpoints repeatedly
- [ ] Respect check interval
- [ ] Save results
- [ ] Log check results
- [ ] Handle exceptions safely
- [ ] Stop gracefully on app shutdown

### Acceptance criteria

- [ ] Worker starts with app
- [ ] Active APIs are checked automatically
- [ ] Disabled APIs are skipped
- [ ] Results are stored in database
- [ ] Worker does not crash app

### Commit

```bash
git commit -m "feat: add background API monitoring worker"
```

---

## AM-036: Add manual check now

### Tasks

- [ ] Add `Check now` button on `/apis`
- [ ] Add `Check now` button on API details/edit page
- [ ] Run health check immediately
- [ ] Save result
- [ ] Show loading state while checking
- [ ] Show result toast/status
- [ ] Disable button while running

### Acceptance criteria

- [ ] User can manually test endpoint
- [ ] Result is saved
- [ ] UI updates after manual check

### Commit

```bash
git commit -m "feat: add manual API check action"
```

---

## AM-037: Add monitoring settings

### Tasks

- [ ] Add configuration object `MonitoringOptions`
- [ ] Bind from appsettings
- [ ] Configure default timeout
- [ ] Configure default interval
- [ ] Configure slow threshold
- [ ] Configure max concurrent checks
- [ ] Validate options on startup

### Acceptance criteria

- [ ] Monitoring behavior is configurable
- [ ] Invalid config fails safely

### Commit

```bash
git commit -m "chore: add monitoring configuration options"
```

---

# 35% - 45%: Dashboard & Real-Time Updates

## EPIC AM-040: Dashboard

**Goal:** Show real-time overview of API health.

**Branch:**

```bash
feature/dashboard
```

---

## AM-041: Add dashboard route

### Route

```text
/dashboard
```

### Tasks

- [ ] Create `Dashboard.razor`
- [ ] Add page header
- [ ] Add loading state
- [ ] Add empty state
- [ ] Add summary cards
- [ ] Add current status table/grid
- [ ] Add recent failures section
- [ ] Add link to create API when no data exists

### Summary cards

- [ ] Total APIs
- [ ] Healthy APIs
- [ ] Slow APIs
- [ ] Down APIs
- [ ] Disabled APIs
- [ ] Average response time

### Acceptance criteria

- [ ] Dashboard loads from real database data
- [ ] Status values are accurate
- [ ] Empty state works

### Commit

```bash
git commit -m "feat: add API monitoring dashboard"
```

---

## AM-042: Add dashboard query service

### Tasks

- [ ] Create `IDashboardService`
- [ ] Add summary DTO
- [ ] Add endpoint status DTO
- [ ] Query latest result per endpoint
- [ ] Calculate counts
- [ ] Calculate average response time
- [ ] Sort by severity
- [ ] Optimize query

### Acceptance criteria

- [ ] Dashboard page does not contain complex query logic
- [ ] Status sorting works
- [ ] Summary values match database

### Commit

```bash
git commit -m "feat: add dashboard summary service"
```

---

## AM-043: Add SignalR hub

### Tasks

- [ ] Create `DashboardHub`
- [ ] Register SignalR
- [ ] Map hub route
- [ ] Define event name:
  - [ ] `ApiCheckCompleted`
  - [ ] `DashboardSummaryUpdated`
- [ ] Send update after worker saves result
- [ ] Handle connected/disconnected clients

### Acceptance criteria

- [ ] Hub route is mapped
- [ ] Worker can send events
- [ ] No build errors

### Commit

```bash
git commit -m "feat: add SignalR dashboard hub"
```

---

## AM-044: Connect dashboard to SignalR

### Tasks

- [ ] Add SignalR client connection in Dashboard page
- [ ] Listen for check result events
- [ ] Update affected endpoint row/card
- [ ] Update summary metrics
- [ ] Dispose connection
- [ ] Handle reconnecting state
- [ ] Show live indicator

### Acceptance criteria

- [ ] Dashboard updates without browser refresh
- [ ] SignalR connection is disposed correctly
- [ ] User sees live status

### Commit

```bash
git commit -m "feat: add live dashboard updates"
```

---

## AM-045: Add status UI components

### Tasks

- [ ] Improve `StatusBadge`
- [ ] Add status dot
- [ ] Add severity sorting helper
- [ ] Add status color mapping
- [ ] Add response time formatting helper
- [ ] Add last checked relative time helper

### Acceptance criteria

- [ ] Healthy/Slow/Down/Unknown/Disabled look consistent
- [ ] UI does not show raw enum values awkwardly
- [ ] Status is accessible by text, not only color

### Commit

```bash
git commit -m "feat: add API status UI components"
```

---

# 45% - 55%: History, Charts, Logs

## EPIC AM-050: Historical Monitoring Data

**Goal:** Users can inspect check history, performance trends, and logs.

**Branch:**

```bash
feature/history-charts-logs
```

---

## AM-051: Add API details page

### Route

```text
/apis/{id}
```

### Tasks

- [ ] Create details page
- [ ] Add breadcrumb
- [ ] Add header with name/status
- [ ] Add overview tab
- [ ] Add history tab
- [ ] Add logs tab
- [ ] Add settings/edit action
- [ ] Show latest check result
- [ ] Show uptime summary
- [ ] Show average response time

### Acceptance criteria

- [ ] Detail page loads endpoint
- [ ] Not found state works
- [ ] Tabs work
- [ ] Data is accurate

### Commit

```bash
git commit -m "feat: add API endpoint details page"
```

---

## AM-052: Add check history table

### Tasks

- [ ] Create history query
- [ ] Add table columns:
  - [ ] Checked time
  - [ ] Status
  - [ ] Status code
  - [ ] Response time
  - [ ] Error message
- [ ] Add pagination
- [ ] Add time range filter
- [ ] Add status filter
- [ ] Add endpoint filter
- [ ] Add loading state
- [ ] Add empty state

### Acceptance criteria

- [ ] History is paginated
- [ ] Filters work
- [ ] Table remains usable with many results

### Commit

```bash
git commit -m "feat: add API check history table"
```

---

## AM-053: Add response time chart

### Tasks

- [ ] Choose chart approach:
  - [ ] Blazorise chart if suitable
  - [ ] Chart.js wrapper if needed
- [ ] Create chart component
- [ ] Query history points
- [ ] Show response time over time
- [ ] Add time range options:
  - [ ] Last hour
  - [ ] Last 24 hours
  - [ ] Last 7 days
- [ ] Show empty chart state
- [ ] Add loading state
- [ ] Make chart responsive

### Acceptance criteria

- [ ] Chart renders real data
- [ ] Time range changes data
- [ ] Chart works on mobile
- [ ] No chart crash when no data exists

### Commit

```bash
git commit -m "feat: add response time chart"
```

---

## AM-054: Add uptime calculation

### Tasks

- [ ] Define uptime formula
- [ ] Add query for successful checks / total checks
- [ ] Add uptime for endpoint
- [ ] Add uptime for dashboard summary
- [ ] Add time range support
- [ ] Format as percentage

### Formula

```text
Uptime % = successful checks / total checks * 100
```

### Acceptance criteria

- [ ] Uptime is calculated correctly
- [ ] Disabled periods are handled consistently
- [ ] Empty data shows Unknown, not 0%

### Commit

```bash
git commit -m "feat: add uptime metrics"
```

---

## AM-055: Add logs page

### Route

```text
/logs
```

### Tasks

- [ ] Create logs page
- [ ] Show check results as operational logs
- [ ] Add filters:
  - [ ] Endpoint
  - [ ] Status
  - [ ] Time range
  - [ ] Search error message
- [ ] Add pagination
- [ ] Add log severity labels
- [ ] Add details drawer/modal
- [ ] Add export placeholder

### Acceptance criteria

- [ ] Logs are filterable
- [ ] Logs are paginated
- [ ] Errors are readable
- [ ] Large logs do not break UI

### Commit

```bash
git commit -m "feat: add monitoring logs page"
```

---

# 55% - 65%: Alerts

## EPIC AM-060: Alerting System

**Goal:** Create alert rules and show active alerts when APIs fail or become slow.

**Branch:**

```bash
feature/alerts
```

---

## AM-061: Add alert domain models

### Tasks

- [ ] Create `AlertRule`
- [ ] Create `AlertEvent`
- [ ] Create `AlertSeverity`
- [ ] Create `AlertConditionType`
- [ ] Add properties:
  - [ ] Endpoint ID
  - [ ] Condition type
  - [ ] Threshold
  - [ ] Duration/failure count
  - [ ] Severity
  - [ ] IsEnabled
  - [ ] CreatedAtUtc
- [ ] Add EF configuration
- [ ] Add migration

### Alert conditions

```text
API is down
API is slow
Response time > threshold
Status code >= 500
Consecutive failures >= N
Uptime below SLA
```

### Acceptance criteria

- [ ] Alert tables created
- [ ] Alert rules can be persisted

### Commit

```bash
git commit -m "feat: add alert domain models"
```

---

## AM-062: Add alert rule service

### Tasks

- [ ] Create `IAlertRuleService`
- [ ] Add CRUD methods
- [ ] Validate alert rules
- [ ] Query enabled rules
- [ ] Register service

### Acceptance criteria

- [ ] Alert rules can be created, edited, deleted
- [ ] Invalid rules are rejected

### Commit

```bash
git commit -m "feat: add alert rule service"
```

---

## AM-063: Add alerts UI

### Route

```text
/alerts
```

### Tasks

- [ ] Add alert rules list
- [ ] Add create alert rule page/modal
- [ ] Add edit alert rule page/modal
- [ ] Add delete confirmation
- [ ] Add enable/disable toggle
- [ ] Add active alerts section
- [ ] Add severity badges

### Acceptance criteria

- [ ] User can manage alert rules
- [ ] Active alerts are visible
- [ ] UI follows design system

### Commit

```bash
git commit -m "feat: add alerts management UI"
```

---

## AM-064: Add alert evaluation engine

### Tasks

- [ ] Create `IAlertEvaluator`
- [ ] Evaluate rules after each check result
- [ ] Detect down alerts
- [ ] Detect slow alerts
- [ ] Detect consecutive failures
- [ ] Avoid duplicate active alerts
- [ ] Resolve alerts when condition clears
- [ ] Store alert events

### Acceptance criteria

- [ ] Alert is created when rule condition is met
- [ ] Alert is not duplicated every check
- [ ] Alert resolves when API recovers

### Commit

```bash
git commit -m "feat: add alert evaluation engine"
```

---

## AM-065: Add dashboard alert summary

### Tasks

- [ ] Show active alert count
- [ ] Show recent critical alerts
- [ ] Link to alerts page
- [ ] Update via SignalR
- [ ] Add toast for new critical alert

### Acceptance criteria

- [ ] Dashboard shows current active alerts
- [ ] New alert appears live

### Commit

```bash
git commit -m "feat: add active alerts to dashboard"
```

---

# 65% - 72%: Authentication & Authorization

## EPIC AM-070: Users and Roles

**Goal:** Protect management actions and support different user roles.

**Branch:**

```bash
feature/auth-roles
```

---

## AM-071: Add authentication

### Tasks

- [ ] Choose authentication approach:
  - [ ] ASP.NET Core Identity for local auth
  - [ ] External provider later if needed
- [ ] Add Identity packages if needed
- [ ] Add user entity/context integration
- [ ] Add login page
- [ ] Add logout
- [ ] Add register page if required
- [ ] Protect app routes
- [ ] Add auth state to layout

### Acceptance criteria

- [ ] Users can log in
- [ ] Users can log out
- [ ] Protected pages require authentication

### Commit

```bash
git commit -m "feat: add user authentication"
```

---

## AM-072: Add roles and policies

### Roles

```text
Admin
Operator
Viewer
```

### Tasks

- [ ] Seed roles
- [ ] Create authorization policies
- [ ] Admin can manage APIs/settings/users
- [ ] Operator can run checks and manage alerts
- [ ] Viewer can read dashboards/logs only
- [ ] Protect UI actions with `AuthorizeView`
- [ ] Protect services/endpoints server-side

### Acceptance criteria

- [ ] Unauthorized users cannot access protected actions
- [ ] UI hides or disables unauthorized actions
- [ ] Server enforces authorization

### Commit

```bash
git commit -m "feat: add role-based authorization"
```

---

## AM-073: Add permission-based UI

### Tasks

- [ ] Hide create/edit/delete API for Viewer
- [ ] Hide settings for non-admin
- [ ] Hide user management for non-admin
- [ ] Show read-only notice when appropriate
- [ ] Add permission empty state

### Acceptance criteria

- [ ] UI reflects current user's permissions
- [ ] Permission messages are clear

### Commit

```bash
git commit -m "feat: add permission-based UI states"
```

---

# 72% - 80%: Environments, Groups, Settings

## EPIC AM-080: Organization Features

**Goal:** Organize APIs by environment, group, tags, and settings.

**Branch:**

```bash
feature/environments-groups-settings
```

---

## AM-081: Add environments

### Tasks

- [ ] Create `ApiEnvironment`
- [ ] Seed:
  - [ ] Development
  - [ ] Staging
  - [ ] Production
- [ ] Add environment to API endpoint
- [ ] Add environment filter
- [ ] Add environment badge
- [ ] Add migration
- [ ] Update create/edit forms

### Acceptance criteria

- [ ] APIs can be assigned to environment
- [ ] Dashboard can filter by environment
- [ ] Environment is visible in tables/cards

### Commit

```bash
git commit -m "feat: add API environments"
```

---

## AM-082: Add API groups/services

### Tasks

- [ ] Create `ApiGroup`
- [ ] Add group to endpoint
- [ ] Add group CRUD
- [ ] Add group filter
- [ ] Add group page or dropdown
- [ ] Show group on dashboard/list

### Acceptance criteria

- [ ] APIs can be grouped by service/team/domain
- [ ] Dashboard supports group filtering

### Commit

```bash
git commit -m "feat: add API grouping"
```

---

## AM-083: Add tags

### Tasks

- [ ] Add tag model
- [ ] Add many-to-many endpoint tags
- [ ] Add tag input
- [ ] Add tag filter
- [ ] Add tag badges

### Acceptance criteria

- [ ] Endpoints can have tags
- [ ] Users can filter by tags

### Commit

```bash
git commit -m "feat: add endpoint tags"
```

---

## AM-084: Add settings page

### Route

```text
/settings
```

### Tasks

- [ ] Add settings page
- [ ] Add monitoring defaults
- [ ] Add slow threshold setting
- [ ] Add timeout setting
- [ ] Add retention period setting
- [ ] Add notification settings placeholder
- [ ] Add theme preference
- [ ] Validate settings
- [ ] Save settings

### Acceptance criteria

- [ ] Settings can be viewed and updated
- [ ] Settings affect monitoring behavior
- [ ] Invalid settings are rejected

### Commit

```bash
git commit -m "feat: add application settings page"
```

---

# 80% - 88%: Security & Reliability

## EPIC AM-090: Hardening

**Goal:** Make the app safe, resilient, and production-ready.

**Branch:**

```bash
feature/security-reliability
```

---

## AM-091: Add request timeout and cancellation

### Tasks

- [ ] Add timeout per request
- [ ] Add cancellation token support
- [ ] Add timeout result classification
- [ ] Add timeout setting in UI/config
- [ ] Test slow API timeout

### Acceptance criteria

- [ ] Slow checks do not hang worker
- [ ] Timeout is recorded as Down
- [ ] App remains responsive

### Commit

```bash
git commit -m "feat: add monitoring timeout handling"
```

---

## AM-092: Add retry policy

### Tasks

- [ ] Add Polly or custom retry
- [ ] Retry only safe methods by default
- [ ] Configure retry count
- [ ] Configure retry delay
- [ ] Log retry attempts
- [ ] Avoid alert spam from transient failure

### Acceptance criteria

- [ ] Transient failures retry safely
- [ ] Retries do not create duplicate check results
- [ ] Retry settings are configurable

### Commit

```bash
git commit -m "feat: add retry policy for API checks"
```

---

## AM-093: Add SSRF protection

### Tasks

- [ ] Validate monitored URLs
- [ ] Require HTTP/HTTPS
- [ ] Optionally block private IP ranges
- [ ] Optionally block localhost/internal metadata endpoints
- [ ] Resolve host safely
- [ ] Add allowlist/denylist settings
- [ ] Add validation errors for blocked URLs

### Acceptance criteria

- [ ] Dangerous internal URLs can be blocked
- [ ] URL validation is server-side
- [ ] Error messages are user-friendly

### Commit

```bash
git commit -m "feat: add URL safety validation"
```

---

## AM-094: Add audit logging

### Tasks

- [ ] Create `AuditLog`
- [ ] Log create/edit/delete endpoint
- [ ] Log enable/disable monitoring
- [ ] Log alert rule changes
- [ ] Log settings changes
- [ ] Include user ID when auth exists
- [ ] Add audit log page for Admin

### Acceptance criteria

- [ ] Sensitive changes are auditable
- [ ] Audit logs do not store secrets

### Commit

```bash
git commit -m "feat: add audit logging"
```

---

## AM-095: Add retention cleanup

### Tasks

- [ ] Add retention setting
- [ ] Create cleanup worker
- [ ] Delete old check results
- [ ] Keep aggregated data if needed
- [ ] Log cleanup result
- [ ] Add tests

### Acceptance criteria

- [ ] Old data is removed based on retention
- [ ] Database does not grow forever

### Commit

```bash
git commit -m "feat: add check result retention cleanup"
```

---

# 88% - 94%: Testing & Quality

## EPIC AM-100: Quality Engineering

**Goal:** Add confidence through tests, QA, accessibility, and code quality.

**Branch:**

```bash
feature/testing-quality
```

---

## AM-101: Add unit test projects

### Tasks

- [ ] Create `tests/ApiMonitor.UnitTests`
- [ ] Add xUnit
- [ ] Add FluentAssertions if desired
- [ ] Add test references
- [ ] Add first smoke test
- [ ] Add test command to README

### Acceptance criteria

- [ ] `dotnet test` runs
- [ ] Test project is in solution

### Commit

```bash
git commit -m "test: add unit test project"
```

---

## AM-102: Test status classification

### Tasks

- [ ] Test Healthy rule
- [ ] Test Slow rule
- [ ] Test Down from status code
- [ ] Test Down from timeout
- [ ] Test Unknown
- [ ] Test Disabled

### Acceptance criteria

- [ ] Status classifier is fully covered

### Commit

```bash
git commit -m "test: add API health status tests"
```

---

## AM-103: Test services

### Tasks

- [ ] Test API endpoint service create
- [ ] Test update
- [ ] Test delete
- [ ] Test validation failure
- [ ] Test dashboard summary service
- [ ] Test alert evaluator

### Acceptance criteria

- [ ] Core business services have tests
- [ ] Tests are deterministic

### Commit

```bash
git commit -m "test: add application service tests"
```

---

## AM-104: Add component tests

### Tasks

- [ ] Add bUnit test project
- [ ] Test `StatusBadge`
- [ ] Test `MetricCard`
- [ ] Test `EmptyState`
- [ ] Test API list page
- [ ] Test form validation display
- [ ] Test permission UI if auth exists

### Acceptance criteria

- [ ] Shared components have tests
- [ ] Main pages render in expected states

### Commit

```bash
git commit -m "test: add Blazor component tests"
```

---

## AM-105: Add integration tests

### Tasks

- [ ] Add integration test project
- [ ] Test EF Core repository/service
- [ ] Test migration/database setup
- [ ] Test health checker with fake HTTP handler
- [ ] Test background worker logic with fake services

### Acceptance criteria

- [ ] Integration tests cover persistence and monitoring behavior

### Commit

```bash
git commit -m "test: add integration tests"
```

---

## AM-106: Add E2E tests

### Tasks

- [ ] Add Playwright
- [ ] Test create API flow
- [ ] Test edit API flow
- [ ] Test delete API flow
- [ ] Test dashboard loads
- [ ] Test responsive/mobile basic flow

### Acceptance criteria

- [ ] Critical user flows are tested in browser

### Commit

```bash
git commit -m "test: add end-to-end UI tests"
```

---

## AM-107: Add accessibility QA

### Tasks

- [ ] Check keyboard navigation
- [ ] Check form labels
- [ ] Check focus states
- [ ] Check color contrast
- [ ] Check modal focus behavior
- [ ] Check screen reader labels
- [ ] Add accessibility checklist to PR template

### Acceptance criteria

- [ ] No obvious accessibility blockers
- [ ] All main workflows are keyboard usable

### Commit

```bash
git commit -m "test: add accessibility QA checklist"
```

---

# 94% - 100%: Production Polish & Deployment

## EPIC AM-110: Release Readiness

**Goal:** Make the app demo-ready and production-ready.

**Branch:**

```bash
feature/release-readiness
```

---

## AM-111: Add production-ready UX polish

### Tasks

- [ ] Apply design system to all pages
- [ ] Replace raw Bootstrap classes where appropriate
- [ ] Ensure consistent spacing
- [ ] Ensure consistent button labels
- [ ] Add toasts for user actions
- [ ] Add confirmation dialogs
- [ ] Add loading skeletons
- [ ] Add friendly error pages
- [ ] Add 404 page
- [ ] Add global error boundary
- [ ] Add dark mode if not already complete

### Acceptance criteria

- [ ] UI looks consistent
- [ ] No unfinished placeholder pages
- [ ] Errors are user-friendly

### Commit

```bash
git commit -m "style: polish application UI"
```

---

## AM-112: Add seed/demo data

### Tasks

- [ ] Add development seed data
- [ ] Add sample APIs:
  - [ ] JSONPlaceholder
  - [ ] GitHub API
  - [ ] Slow test endpoint
  - [ ] Error test endpoint
- [ ] Add seed command or dev-only seeding
- [ ] Ensure seed does not run in production accidentally

### Acceptance criteria

- [ ] New developer can see dashboard with sample data
- [ ] Seed is development-safe

### Commit

```bash
git commit -m "chore: add development seed data"
```

---

## AM-113: Add Docker support

### Tasks

- [ ] Add `Dockerfile`
- [ ] Add `.dockerignore`
- [ ] Add `docker-compose.yml`
- [ ] Add SQL Server container or use external DB
- [ ] Add environment variables
- [ ] Document Docker run command

### Acceptance criteria

- [ ] App can run with Docker
- [ ] README explains Docker setup

### Commit

```bash
git commit -m "chore: add Docker support"
```

---

## AM-114: Add CI pipeline

### Tasks

- [ ] Add GitHub Actions or preferred CI
- [ ] Restore packages
- [ ] Build solution
- [ ] Run tests
- [ ] Publish artifact
- [ ] Optional: run Playwright tests
- [ ] Optional: run formatting check

### Acceptance criteria

- [ ] CI runs on pull requests
- [ ] Build/test failures block merge

### Commit

```bash
git commit -m "chore: add CI pipeline"
```

---

## AM-115: Add deployment guide

### Tasks

- [ ] Document environment variables
- [ ] Document connection strings
- [ ] Document migrations
- [ ] Document production logging
- [ ] Document HTTPS requirement
- [ ] Document reverse proxy notes
- [ ] Document troubleshooting

### Acceptance criteria

- [ ] Another developer can deploy the app from docs

### Commit

```bash
git commit -m "docs: add deployment guide"
```

---

## AM-116: Add final README

### Tasks

- [ ] Add project description
- [ ] Add screenshots/GIF placeholders
- [ ] Add feature list
- [ ] Add architecture overview
- [ ] Add setup steps
- [ ] Add run commands
- [ ] Add test commands
- [ ] Add roadmap
- [ ] Add tech stack
- [ ] Add license if needed

### Acceptance criteria

- [ ] README is portfolio-ready
- [ ] README explains why project is impressive

### Commit

```bash
git commit -m "docs: complete project README"
```

---

# Optional Pro-Level Features After 100%

These are not required for the first complete version, but they can make the project stand out strongly.

---

## PRO-001: Public status page

### Tasks

- [ ] Add public `/status` page
- [ ] Show public APIs only
- [ ] Hide private URLs if configured
- [ ] Show uptime summary
- [ ] Show current incidents
- [ ] Add no-auth access

### Branch

```bash
feature/public-status-page
```

---

## PRO-002: Incident timeline

### Tasks

- [ ] Create incident model
- [ ] Detect outage start/end
- [ ] Show incident duration
- [ ] Add incident notes
- [ ] Add incident timeline page
- [ ] Link incidents to alerts

### Branch

```bash
feature/incidents
```

---

## PRO-003: SLA reporting

### Tasks

- [ ] Add SLA target per endpoint/group
- [ ] Calculate SLA compliance
- [ ] Show monthly report
- [ ] Export report
- [ ] Show SLA violations

### Branch

```bash
feature/sla-reporting
```

---

## PRO-004: Maintenance windows

### Tasks

- [ ] Add maintenance window model
- [ ] Disable alerts during maintenance
- [ ] Show maintenance badge
- [ ] Exclude maintenance from SLA if configured
- [ ] Add calendar/list UI

### Branch

```bash
feature/maintenance-windows
```

---

## PRO-005: Multi-tenant support

### Tasks

- [ ] Add Tenant model
- [ ] Add tenant ID to entities
- [ ] Add global query filters
- [ ] Add tenant switching
- [ ] Add tenant-specific roles
- [ ] Add tenant isolation tests

### Branch

```bash
feature/multi-tenant-support
```

---

# Recommended Build Order From Your Current State

If you already completed solution setup and basic API CRUD, continue like this:

```text
1. Finish and merge feature/api-endpoint-crud
2. Start feature/blazorise-ui-foundation
3. Refactor current CRUD pages to Blazorise
4. Start feature/api-monitoring-worker
5. Start feature/dashboard
6. Start feature/history-charts-logs
7. Start feature/alerts
8. Add auth/roles
9. Add environments/groups/settings
10. Add security, tests, and deployment
```

---

# Branch Plan Summary

| Order | Branch | Purpose |
|---:|---|---|
| 1 | `feature/project-foundation` | Solution setup |
| 2 | `feature/blazorise-ui-foundation` | Design system and Blazorise setup |
| 3 | `feature/api-endpoint-crud` | API CRUD |
| 4 | `feature/api-monitoring-worker` | Background checks |
| 5 | `feature/dashboard` | Dashboard and live status |
| 6 | `feature/history-charts-logs` | History, charts, logs |
| 7 | `feature/alerts` | Alert rules and active alerts |
| 8 | `feature/auth-roles` | Authentication and authorization |
| 9 | `feature/environments-groups-settings` | Organization and settings |
| 10 | `feature/security-reliability` | Security and hardening |
| 11 | `feature/testing-quality` | Tests and QA |
| 12 | `feature/release-readiness` | Deployment and polish |

---

# Definition of Done For Every Feature

A feature is done only when:

- [ ] Code builds with `dotnet build`
- [ ] App runs locally
- [ ] UI follows design system
- [ ] Loading state exists where needed
- [ ] Empty state exists where needed
- [ ] Error state exists where needed
- [ ] Validation works where needed
- [ ] No secrets are committed
- [ ] No sensitive data is logged
- [ ] Feature has at least basic tests if logic is important
- [ ] README/docs updated if behavior changed
- [ ] Branch has clean commits
- [ ] Feature is merged only after testing

---

# Minimum MVP Scope

If you want the smallest impressive version, build only this:

```text
1. Blazorise UI foundation
2. API CRUD
3. Background monitoring worker
4. Save check results
5. Dashboard
6. SignalR live updates
7. Response time chart
8. Logs page
9. Basic alerts
```

That MVP is already strong enough for a portfolio or interview demo.

---

# Full 100% Feature Scope

The complete production-grade version includes:

```text
API CRUD
Blazorise design system
Background monitoring
Manual checks
Status classification
Response time tracking
Check history
Dashboard
SignalR live updates
Charts
Logs
Filtering
Pagination
Alerts
Alert rules
Active alert lifecycle
Notifications
Authentication
Authorization
Roles
Environments
Groups
Tags
Settings
Audit logs
Security validation
SSRF protection
Retries
Timeouts
Retention cleanup
Unit tests
Component tests
Integration tests
E2E tests
Accessibility checks
Docker
CI/CD
Deployment docs
Production polish
```