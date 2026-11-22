# QA-OPS Playwright

An end-to-end test automation repository built with Playwright, JavaScript, TypeScript, Cucumber, and Playwright API capabilities. It contains browser UI tests, API and network-interception examples, page-object implementations, custom Playwright fixtures, Postman collections, and Azure Playwright Service configuration.

The samples exercise public training applications hosted by Rahul Shetty Academy and are intended as reusable QA learning and automation examples.

## Repository at a Glance

| Area | Description |
|---|---|
| `tests/` | Playwright test specifications in JavaScript and TypeScript |
| `pageobjects/` | JavaScript Page Object Model implementations |
| `pageobjects_ts/` | TypeScript Page Object Model implementations |
| `utils/` | JavaScript fixtures, API helpers, test data, and test fixtures |
| `utils_ts/` | TypeScript API helpers, test data, and test fixtures |
| `features/` | Cucumber feature files |
| `features/step_definitions/` | JavaScript and TypeScript Cucumber step definitions |
| `features/support/` | Cucumber support and hook files |
| `postmanCollection.json` | Exported Postman collection for the e-commerce API |
| `playwright.config.js` | Default local Playwright configuration |
| `playwright.config1.js` | Safari and Chrome project configuration |
| `playwright.service.config.js` | Azure Playwright Service configuration |
| `.github/workflows/playwright.yml` | GitHub Actions CI workflow |

## Technologies

- Node.js and npm
- Playwright Test
- JavaScript and TypeScript
- Cucumber
- Playwright API testing and network routing
- ExcelJS
- HTML, line, and Azure Playwright test reporting
- Azure Playwright Service
- GitHub Actions

## Prerequisites

- Node.js LTS
- npm
- Internet access to the public training applications
- A supported Playwright browser installed locally

## Installation

```powershell
npm ci
npx playwright install
```

On Linux CI runners, install the Playwright system dependencies with:

```bash
npx playwright install --with-deps
```

## Running Tests

Run the complete Playwright suite with the default configuration:

```powershell
npm run regression
```

Run Web-tagged tests:

```powershell
npm run webTests
```

Run API-tagged tests:

```powershell
npm run APITests
```

Run the Safari project from the alternate configuration:

```powershell
npm run SafariNewConfig
```

Run a specific spec:

```powershell
npx playwright test tests/UIBasicstest.spec.js
```

Run all tests in a headed browser:

```powershell
npx playwright test --headed
```

Run tests with Chromium, Firefox, or WebKit explicitly:

```powershell
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit
```

Open the last HTML report:

```powershell
npx playwright show-report
```

## Playwright Configurations

### Default configuration

`playwright.config.js` applies these settings:

- Test directory: `./tests`
- Test pattern: `**/*.spec.js`
- Browser: Chromium
- Mode: headless
- Screenshots: on
- Trace: on
- Timeout: 30 seconds
- Expect timeout: 5 seconds
- Retries: 0
- HTML reporter

### Alternate configuration

`playwright.config1.js` defines Safari and Chrome projects:

- `safari`: WebKit with iPhone 11 device settings
- `chrome`: Chromium with video retained on failure
- Workers: 3
- Retries: 1
- 30-second test timeout

Use it with:

```powershell
npx playwright test --config=playwright.config1.js
npx playwright test --config=playwright.config1.js --project=safari
npx playwright test --config=playwright.config1.js --project=chrome
```

### Azure Playwright Service configuration

`playwright.service.config.js` combines the default configuration with `@azure/playwright`. It uses:

- Azure `DefaultAzureCredential`
- Linux service OS
- Loopback network exposure
- HTML and Azure Playwright reporters

Set `PLAYWRIGHT_SERVICE_URL` and Azure credentials before running service-backed tests.

## Test Coverage

The suite demonstrates a broad range of Playwright and QA concepts.

| Area | Examples |
|---|---|
| Login and validation | Valid and invalid login flows, UI controls, and error handling |
| E-commerce flow | Login, product search, cart, checkout, order placement, and order history |
| Page Object Model | JavaScript and TypeScript page classes managed by `POManager` |
| API testing | Login, order creation, and authenticated API workflows |
| Network testing | Request interception and response routing |
| Network mocking | Mocking order-history or order-detail responses |
| Locators | Standard locators and Playwright special locators |
| Browser contexts | Multiple browser contexts and child-window handling |
| Test fixtures | Shared authenticated pages and API-created orders |
| File upload/download | Excel upload and download validation |
| Popups and dialogs | Popup and alert validation |
| Visual testing | Screenshots and visual comparison |
| Calendars | Calendar date selection |
| Cucumber | Behavior-driven feature files and step definitions |

## JavaScript and TypeScript Examples

JavaScript and TypeScript versions coexist in several areas:

- `tests/ClientAppPO.spec.js` and `tests/ClientAppPO.spec.ts`
- `pageobjects/` and `pageobjects_ts/`
- `utils/` and `utils_ts/`
- Cucumber JavaScript and TypeScript step definitions

`tsconfig.json` enables strict TypeScript checking and CommonJS module output.

## Page Object Model

The JavaScript POM is centered on `pageobjects/POManager.js`, which creates and provides:

- `LoginPage`
- `DashboardPage`
- `CartPage`
- `OrdersReviewPage`
- `OrdersHistoryPage`

The same pattern is implemented in `pageobjects_ts/` for TypeScript tests.

Typical usage:

```javascript
const { POManager } = require('../pageobjects/POManager');

const poManager = new POManager(page);
const loginPage = poManager.getLoginPage();
await loginPage.goTo();
await loginPage.validLogin(username, password);
```

## Fixtures and Test Data

`utils/test-base.js` adds a custom test fixture containing order credentials and product data.

`utils/fixtures.js` demonstrates fixtures for:

- An authenticated browser page
- An API-created order
- Order test data

Test data is stored in JSON files under `utils/` and `utils_ts/`. Some credentials and payloads are also defined directly in test and utility files. They are training/sample values; do not reuse real secrets in this repository.

## API Testing

`utils/APiUtils.js` and `utils_ts/APiUtils.ts` provide helpers that:

1. Authenticate against the e-commerce API.
2. Retrieve an authentication token.
3. Create an order with an authenticated API request.
4. Return the created order ID for use in UI tests.

The export `postmanCollection.json` contains the corresponding Login and Create Order requests.

## Cucumber / BDD

Feature files are located in `features/`.

Run a feature:

```powershell
npx cucumber-js features/greeting.feature --exit
```

Run in parallel:

```powershell
npx cucumber-js --parallel 2 --exit --format html:cucumber-report.html
```

`cucumber.js` disables Cucumber's published output noise through `--publish-quiet`.

## CI/CD

`.github/workflows/playwright.yml` runs Playwright on pull requests and pushes to `main` or `master`.

The workflow:

1. Checks out the repository.
2. Uses the current Node.js LTS version.
3. Runs `npm ci`.
4. Authenticates to Azure with `AZURE_CREDENTIALS`.
5. Runs tests through `playwright.service.config.js`.
6. Uploads `playwright-report/` as a build artifact for 30 days.

Required GitHub configuration:

- Secret: `AZURE_CREDENTIALS`
- Variable: `PLAYWRIGHT_SERVICE_URL`

## Reports and Artifacts

The default and alternate configurations can generate Playwright HTML reports, traces, screenshots, and failure videos.

Generated local directories include:

- `playwright-report/`
- `test-results/`
- `allure-results/`
- `allure-report/`

These directories are excluded by `.gitignore`.

## Security and Sensitive Data

Do not commit authentication tokens, API keys, Azure credentials, or real production passwords.

Before reuse in a shared environment:

- Move credentials to environment variables or GitHub Secrets.
- Replace hard-coded sample credentials in test data and utilities.
- Add generated reports and screenshots to `.gitignore`.
- Regenerate any exposed Postman or API token value.

## Troubleshooting

### Browser not found

Run:

```powershell
npx playwright install
```

### Missing Linux system dependencies

Run:

```bash
npx playwright install --with-deps
```

### Network or external application failure

Confirm internet access and that the following training hosts are reachable:

- `rahulshettyacademy.com`

### Azure Playwright Service failure

Confirm that:

- `AZURE_CREDENTIALS` is valid.
- `PLAYWRIGHT_SERVICE_URL` is configured.
- The Azure identity has access to the Playwright workspace.

### Stale reports or test state

Remove generated result directories if needed:

```powershell
Remove-Item -Recurse -Force playwright-report, test-results, allure-results, allure-report
```
