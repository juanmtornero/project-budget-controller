# Project Budget Controller

**An interactive, AI-assisted project cost monitoring tool for Project Leads and Project Controllers.**

**[Live demo](https://juanmtornero.github.io/project-budget-controller/)** · [Claude Artifact prototype](https://claude.ai/artifact/XgiSCJHyngAPM5k8LMSR31)

## The business problem

Project Leads often need to open multiple spreadsheets and calculate budget consumption, remaining costs, and potential overruns manually. This makes quick budget reviews repetitive and increases the chance of inconsistent calculations.

Project Budget Controller brings those indicators into a simple interface. Users enter basic project information and see the budget impact immediately.

## What the tool does

Users can enter:

- Project name (optional)
- Total cost budget
- Costs already recorded
- Project completion percentage
- Estimate to Complete (ETC), when available (optional)
- Expected cost profile: linear, front-loaded, or back-loaded

The application displays:

- Percentage of budget consumed
- Budget remaining before future costs
- Estimate at Completion (EAC), when calculable
- Budget variance in euros and percentage
- Recorded versus estimated future costs
- A contextual status message and visual indicators

Values update as users edit the inputs. The prototype does not save projects or provide multi-project reporting.

## Calculation methodology

### When the Project Lead provides an ETC

The manually supplied estimate is used:

```text
EAC = Recorded costs + ETC
```

### When the ETC is not supplied

For projects with a completion percentage between 0% and 100%, the application can generate an indicative projection using a selected cost curve:

```text
Expected cost share = (Completion percentage)^k
EAC = Recorded costs / Expected cost share
```

The tool uses the following illustrative exponents:

| Profile | k | Interpretation |
| --- | ---: | --- |
| Linear | 1.0 | Costs develop in proportion to completion. |
| Front-loaded | 0.6 | A greater share of costs is expected early. |
| Back-loaded | 1.6 | A greater share of costs is expected later. |

**These exponents are generic assumptions, not coefficients calibrated to historical company data.** The automatic EAC is a scenario estimate, not a validated financial forecast. A project-specific estimate of the remaining work is preferable when available.

### Budget variance

The application's favorable/unfavorable convention is:

```text
Budget variance = Budget - EAC
```

- **Positive** variance: favorable, estimated cost below budget.
- **Negative** variance: unfavorable, estimated cost above budget.

At 100% completion, the interface switches from projected to final labels and uses recorded costs as the closing amount. In actual project governance, the result should only be treated as final after confirming that all costs have been recorded.

At 0% completion, the app does not use the automatic division-based projection. An ETC is required to produce an EAC; otherwise, the EAC is unavailable.

If the project is in progress (above 0% and below 100%) but no recorded costs or manual ETC are available, the tool also withholds the EAC instead of assuming the remaining budget is a reliable forecast.

When an automatic EAC is displayed, the interface identifies it as an **indicative projection based on assumptions** and recommends checking actual remaining costs. A manually entered ETC is explicitly attributed to the Project Lead.

## Development process

The prototype was developed through an iterative, AI-assisted workflow:

1. **Business requirements:** Defined the target user, input fields, financial metrics, and alert requirements based on Project Control tasks.
2. **Initial prototype:** Used Claude Artifacts to create a live budget-monitoring interface.
3. **Model review:** Questioned the method for estimating costs under different spending profiles and examined the assumptions behind the exponent-based model.
4. **Business validation:** Identified misleading behavior for projects with upfront costs and no progress, and distinguished estimates from recorded final costs.
5. **Refinement:** Requested clearer favorable/unfavorable terminology, input validation, and special handling of 0% and 100% completion.
6. **Functional testing:** Tested representative budget scenarios, edge cases, and invalid inputs, including missing values and invalid numeric formats.
7. **Publication:** Exported the standalone HTML artifact and published the application using GitHub Pages.

**My contribution:** Business-problem definition, functional requirements, financial interpretation, feedback on the AI-generated solution, and scenario-based testing. Claude generated and iterated the application implementation.

## Manual test cases

The following scenarios were reported as passing during the development exercise:

| Scenario | Budget | Recorded costs | Completion | ETC | Expected outcome |
| --- | ---: | ---: | ---: | ---: | --- |
| Not started, no estimate | €100,000 | €0 | 0% | Not provided | EAC unavailable |
| Upfront costs, manual forecast | €100,000 | €40,000 | 0% | €50,000 | EAC €90,000 |
| In progress, manual forecast | €100,000 | €60,000 | 50% | €50,000 | EAC €110,000 |
| Completed project | €100,000 | €85,000 | 100% | Not applicable | Recorded final cost €85,000 |
| In progress, no recorded costs | €100,000 | €0 | 40% | Not provided | EAC unavailable; request ETC or verify recorded costs |

**Input validation tested:** Negative values, nonnumeric inputs, empty mandatory fields, invalid completion percentages, and the 0%/100% completion edge cases were manually checked during development. These are manual checks rather than an automated test suite or an independent audit.

**Before production use:** Add automated regression tests, independently review the calculation logic, verify accessibility and cross-browser behavior, and validate the forecasting model with real project cost histories.

## Limitations

- Cost curves are illustrative and are not calibrated to any organization's actual spending patterns.
- Completion percentages are user-provided and are not independently verified.
- The application does not model billing milestones, cash collections, recognized revenue, or profitability.
- A completed work percentage does not guarantee all supplier invoices and expenses are recorded.
- The current prototype handles one project at a time and does not store historical data.
- Financial decisions should be supported by a reviewed Estimate to Complete and the project's latest cost baseline.
- The application runs entirely in the browser; it does not connect to a corporate financial system or persist input data.

## Potential next steps

- Optional configuration for known upfront costs or a project-specific spending baseline.
- Import and analyze multiple projects from a spreadsheet.
- Save project scenarios and compare estimate revisions over time.
- Export a short, reviewable project financial summary.

## Tools and context

- **Claude Artifacts:** AI-assisted application creation and iteration.
- **Project Control / financial analysis:** Requirements and review of budgeting logic.
- **Scenario-based QA:** Testing of calculations, edge cases, and user-facing status messages.
- **GitHub Pages:** Hosting the public demonstration.

This is a learning and portfolio project using fictional examples, not a production-grade financial management system.
