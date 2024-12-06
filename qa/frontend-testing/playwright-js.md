# ▶ Playwright

#### links

- FAQ - [https://applitools.com/blog/top-playwright-questions-answered](https://applitools.com/blog/top-playwright-questions-answered/)
- Tutor - [https://testautomationu.applitools.com/js-playwright-tutorial](https://testautomationu.applitools.com/js-playwright-tutorial)
- Basics - [https://testautomationu.applitools.com/playwright-intro/](https://testautomationu.applitools.com/playwright-intro/)
- Webinar - [https://applitools.com/event/playwright-a-new-test-automation-framework-for-the-modern-web](https://applitools.com/event/playwright-a-new-test-automation-framework-for-the-modern-web/)
- basic info about project and examples - [https://www.youtube.com/watch?v=ttODF00XWis\&list=PLPPEvQlTaJs2fm0m3586X3cjC7VGH0ThV](https://www.youtube.com/watch?v=ttODF00XWis\&list=PLPPEvQlTaJs2fm0m3586X3cjC7VGH0ThV)

## basics

Playwright is using Chrome DevTools Protocol (CDP) to communicate with Chromium. For Firefox (Juggler) and Safari (Webinspector Protocol) implemented new protocols similar to the chrome one.

Installs browsers to shared system folder and reuses it for all projects.

#### quick init

```bash
# you need node.js (npm)
# Run from your project's root directory
npm init playwright@latest

# Or create a new project
npm init playwright new-project

# Or install manualy 
npm i -D @playwright/test

# install supported browsers
npx playwright install

# basic commands
npx playwright test # Runs the end-to-end tests.
npx playwright test --ui # Starts the interactive UI mode.
npx playwright test --project=chromium # Runs the tests on Desktop Chrome
npx playwright test example-folder # Runs the tests in a specific file
npx playwright test --debug # Runs the tests in debug mode.
npx playwright codegen # Auto generate tests with Codegen.
```

#### helper

```bash
# check that browsers are installed
npx playwright install
```

#### file structure

`./utils` - additional data, reusable functions
`./tests/example.spec.ts` - Example end-to-end test
`./playwright.config.ts` - Playwright configuration
`.env` - environmental vars

## runner

### test

```bash
# run all
npx playwright test

# run one
npx playwright test tests/example.test.spec.ts

# Runs the tests only on Desktop Chrome.
npx playwright test --project=chromium

# Runs the tests in debug mode.
npx playwright test --debug

# list all tests
npx playwright test --list

# run by test/suite name
npx playwright test --config=./tests/e2e/config.ts -g "Validation fails for invalid card number" --repeat-each 100

# results
npx playwright show-report

# options
## -x # stop on fail
## --retries=10
## --timeout 10000
## --project Desktop\ Chrome
## prod-api/aaa/bbb-exclusive.e2e.ts:70 # start test on line 70
```

## config

`playwright.config.ts`

- docs - https://playwright.dev/docs/test-configuration
- [config example](https://github.com/raptatinha/tau-introduction-to-playwright/blob/chapter-2/playwright.config.ts)

```javascript
import { defineConfig, devices } from '@playwright/test';
import baseEnvUrl from './utils/environmentBaseUrl';

require('dotenv').config();

export default defineConfig({
  testDir: './tests',

  fullyParallel: true, // Run tests in files in parallel

  // Fail the build on CI if you accidentally left test.only in the source code
  forbidOnly: !!process.env.CI,

  /* Retry on CI only */
  // retries: process.env.CI ? 2 : 0,
  retries: 2,

  /* Opt out of parallel tests on CI. */
  workers: process.env.CI ? 1 : undefined,

  /* Reporter to use. See https://playwright.dev/docs/test-reporters */
  reporter: 'html',
  // reporter: [['html', { open: 'always' }]], //always, never and on-failure (default).
  // reporter: [['html', { outputFolder: 'my-report' }]], // report is written into the playwright-report folder in the current working directory. override it using the PLAYWRIGHT_HTML_REPORT
  // reporter: 'dot',
  // reporter: 'list',
  /**
    reporter: [
      ['list'],
      ['json', {  outputFile: 'test-results.json' }]
    ],
  */
  /**
   * custom reports: https://playwright.dev/docs/test-reporters#custom-reporters 
  */
  
  /* Shared settings for all the projects below. See https://playwright.dev/docs/api/class-testoptions. */
  use: {
    /* Base URL to use in actions like `await page.goto('/')`. */
    // baseURL: 'http://127.0.0.1:3000',

    /* Collect trace when retrying the failed test. See https://playwright.dev/docs/trace-viewer */
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    // headless: false,
    // ignoreHTTPSErrors: true,
    // viewport: { width: 1280, height: 720 },
    // video: 'on-first-retry',
  },
    // timeout: 30000, //https://playwright.dev/docs/test-timeouts
    // expect: {
      /**
       * Maximum time expect() should wait for the condition to be met.
       * For example in `await expect(locator).toHaveText();`
       */
      // timeout: 10000,
    // },

  /* Folder for test artifacts such as screenshots, videos, traces, etc. */
  // outputDir: 'test-results/',

  /* Configure projects for major browsers */
  projects: [
    {
      name: 'chromium',
      use: { 
        ...devices['Desktop Chrome'],
        // viewport: { width: 1280, height: 720 },
      },
    },

    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },

    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },

    {
      name: 'all-browsers-and-tests',
      use: { 
        baseURL: 'https://playwright.dev/',
         ...devices['Desktop Chrome']
      },
    },

    {
      name: 'all-browsers-and-tests',
      use: { 
        baseURL: 'https://playwright.dev/',
         ...devices['Desktop Safari']
      },
    },

    {
      name: 'all-browsers-and-tests',
      use: { 
        baseURL: 'https://playwright.dev/',
         ...devices['Desktop Firefox']
      },
    },

    // Example only
    {
      name: 'local',
      use: { 
        baseURL: baseEnvUrl.local.home,
      },
    },

    // Example only
    {
      name: 'ci',
      use: { 
         baseURL: process.env.CI
          ? baseEnvUrl.ci.prefix + process.env.GITHUB_REF_NAME + baseEnvUrl.ci.suffix //https://dev-myapp-chapter-2.mydomain.com
          : baseEnvUrl.staging.home,
      },
      /**
       * GitHub variables: https://docs.github.com/en/actions/learn-github-actions/variables
       * GitLab variables: https://docs.gitlab.com/ee/ci/variables/predefined_variables.html#predefined-variables-reference
       */
    },

    /* Test against mobile viewports. */
    // {
    //   name: 'Mobile Chrome',
    //   use: { ...devices['Pixel 5'] },
    // },
    // {
    //   name: 'Mobile Safari',
    //   use: { ...devices['iPhone 12'] },
    // },

    /* Test against branded browsers. */
    // {
    //   name: 'Microsoft Edge',
    //   use: { ...devices['Desktop Edge'], channel: 'msedge' },
    // },
    // {
    //   name: 'Google Chrome',
    //   use: { ..devices['Desktop Chrome'], channel: 'chrome' },
    // },
  ],

  /* Run your local dev server before starting the tests */
  // webServer: {
  //   command: 'npm run start',
  //   url: 'http://127.0.0.1:3000',
  //   reuseExistingServer: !process.env.CI,
  // },
});
```

## tests

### network monitoring

```js
// Subscribe to 'request' and 'response' events.  
page.on('request', request => console.log('>>', request.method(), request.url()));  
page.on('response', response => console.log('<<', response.status(), response.url()));  
  
await page.goto('https://example.com');
```

### waiting

```js
// wait for resp
await page.waitForResponse(response => response.url().startsWith('https://ponse.res/api/pay/'));
```

### parallel runs

```js
test.describe.configure({ mode: 'parallel' });
```

### code snippets

```javascript
const { chromium } = require('playwright');
// https://playwright.dev/docs/api/class-elementhandle#element-handle-type

( async () => { // => function 
    // launching browser
    const browser = await chromium.launch({ headless: false, slowMo: 100});
    const page = await browser.newPage();
    await page.goto('https://the-internet.herokuapp.com/forgot_password');
    // code to type in email textbox
    const email = await page.$('#email');
    // delaying typing 50 ms to simulate real user speed typing
    await email.type('ixchel@mail.com', { delay : 50});
    await page.click('#smthg');
    await browser.close();
})();
```

```javascript
import { test, expect } from '@playwright/test'; 

test('has title', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  // Expect a title "to contain" a substring.
  await expect(page).toHaveTitle(/Playwright/);
});

test('get started link', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  // Click the get started link.
  await page.getByRole('link', { name: 'Get started' }).click();

  // Expects the URL to contain intro.
  await expect(page).toHaveURL(/.*intro/);
});
```

template example with AAA patern

```javascript
import { test, expect } from '@playwright/test';

//AAA Pattern
// [Arrange]
// [Act]
// [Assert]

const password = process.env.PASSWORD;

test.beforeAll(async ({ playwright }) => {
    test.skip(
      !!process.env.PROD,
      'Test intentionally skipped in production due to data dependency.'
    );
    // start a server
    // create a db connection
    // reuse a sign in state
});
  
test.beforeEach(async ({ page }, testInfo) => {
    console.log(`Running ${testInfo.title}`);
    // open a URL
    // clean up the DB
    // create a page object
    // dismiss a modal
    // load params
});

test.afterAll(async ({ page }, testInfo) => {
    console.log('Test file completed.');
    // close a DB connection
});

test.afterEach( async ({ page }, testInfo) => {
    console.log(`Finished ${testInfo.title} with status ${testInfo.status}`);

    if (testInfo.status !== testInfo.expectedStatus)
        console.log(`Did not run as expected, ended up at ${page.url()}`);
    // clean up all the data we created for this test through API calls
});

test.describe.skip('Test Case', () => {
    test('Test Scenario One', async ({ page }) => {
        await test.step('Step One', async () => {
            // ...
        });

        await test.step('Step Two', async () => {
            // ...
        });

        // ...
    });
  
    test('Test Scenario Two', async ({ page }) => {
        // Arrange
        // Act
        // Assert
    });

    // test.only('Test Scenario Three', async ({ p
  });
```

### built in functions

#### test

takes two arguments: a string representing the title of the test, and an async function that defines the test itself. The async function receives an object with fixtures and optional TestInfo as its argument.

methods

`test.describe` is a method that declares a group of tests.\
It takes two arguments: a string that represents the title of the group, and the second argument is a callback function that contains all the tests belonging to this group. When `test.describe` is called, it immediately runs the callback function and any tests added in this callback will belong to this group.

```javascript
test.describe('Pay button appearance', () => { // declare group of tests
  test('Pay button is displaying if Pay Enabled', async ({ // declare test name
    page, // recieve page object
    request, // recieve request object
  }) => {
    const PayWidget = new PayWidgetComponent(page); // get page elements for test

    await openPayPage(page, request, token, { // open page with test data
      Id: TestId,
    });
    
    await expect( // assertion function
      PayWidget.PayButton, // component
      "I don't see the Pay button" // custom expect message
    ).toBeVisible(); // matcher for assertion 
    await expect(
      PayWidget.ButtonSpmPreview,
      'I can see Pay SPM preview on button'
    ).toBeHidden();
  });
```

#### expect

`expect()`

To make an assertion, call `expect(value)` and choose a matcher that reflects the expectation. There are many generic matchers like `toEqual`, `toContain`, and `toBeTruthy` that can be used to assert any conditions.\
[https://playwright.dev/docs/test-assertions](https://playwright.dev/docs/test-assertions)

### POM

#### structure

```
../pages/home-page
../pages/top-menu-page

../tests/test.spec.ts
```

example test

```javascript
import { test, type Page } from '@playwright/test';
import { HomePage } from '../pages/home-page';
import { TopMenuPage } from '../pages/top-menu-page';

const URL = 'https://playwright.dev/';
let homePage: HomePage;
let topMenuPage: TopMenuPage;
const pageUrl = /.*intro/;

test.beforeEach(async ({page}) => {
    await page.goto(URL);
    homePage = new HomePage(page);
});

async function clickGetStarted(page: Page) {
    await homePage.clickGetStarted();
    topMenuPage = new TopMenuPage(page);
}

test.describe('Playwright website', () => {

    test('has title', async () => {
        await homePage.assertPageTitle();
    });
    
    test('get started link', async ({ page }) => {
        // Act
        await clickGetStarted(page);
        // Assert
        await topMenuPage.assertPageUrl(pageUrl);
    });
    
    test('check Java page', async ({ page }) => {
        await test.step('Act', async () => {
            await clickGetStarted(page);
            await topMenuPage.hoverNode();
            await topMenuPage.clickJava();
        });
      
        await test.step('Assert', async () => {
            await topMenuPage.assertPageUrl(pageUrl);
            await topMenuPage.assertNodeDescriptionNotVisible();
            await topMenuPage.assertJavaDescriptionVisible();
        });
    });
});
```

example pom

```javascript
import { type Locator, type Page, expect } from '@playwright/test';

export class HomePage {
    readonly page: Page;
    readonly getStartedButton: Locator;
    readonly pageTitle: RegExp;

    constructor(page: Page) {
        this.page = page;
        this.getStartedButton = page.getByRole('link', { name: 'Get started' });
        this.pageTitle = /Playwright/;
    }

    async clickGetStarted() {
        await this.getStartedButton.click();
    }

    async assertPageTitle() {
        await expect(this.page).toHaveTitle(this.pageTitle);
    }
}

export default HomePage;
```

one more

```javascript
import { expect, Locator, Page } from '@playwright/test';

export class TopMenuPage {
    readonly page: Page;
    readonly getStartedLink: Locator;
    readonly nodeLink: Locator;
    readonly javaLink: Locator;
    readonly nodeLabel: Locator;
    readonly javaLabel: Locator;
    readonly nodeDescription: string = 'Installing Playwright';
    readonly javaDescription: string = `Playwright is distributed as a set of Maven modules. The easiest way to use it is to add one dependency to your project's pom.xml as described below. If you're not familiar with Maven please refer to its documentation.`;

    constructor(page: Page) {
        this.page = page;
        this.getStartedLink = page.getByRole('link', { name: 'Get started' });
        this.nodeLink = page.getByRole('button', {name: 'Node.js'});
        this.javaLink = page.getByRole('navigation', { name: 'Main' }).getByText('Java');
        this.nodeLabel = page.getByText(this.nodeDescription, {exact:true});
        this.javaLabel = page.getByText(this.javaDescription);
    }

    async hoverNode() {
        await this.nodeLink.hover();
    }
    
    async clickJava() {
        await this.javaLink.click();
    }

    async assertPageUrl(pageUrl: RegExp) {
        await expect(this.page).toHaveURL(pageUrl);
    }

    async assertNodeDescriptionNotVisible() {
        await expect(this.nodeLabel).not.toBeVisible();
    }

    async assertJavaDescriptionVisible() {
        await expect(this.javaLabel).toBeVisible();
    }

}
export default TopMenuPage;
```
