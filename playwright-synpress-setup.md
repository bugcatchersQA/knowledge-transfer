# Initial setup for Playwright framework with Synpress library on top.

## Table of Contents

-   [Description](#description)
-   [Installing Node.js and npm](#installing-nodejs-and-npm)
-   [Installing yarn package manager](#installing-yarn-package-manager)
-   [New Node project](#new-node-project)
-   [Playwright installation](#playwright-installation)
-   [Synpress installation](#synpress-installation)

## Description

Initial setup for Playwright framework with Synpress library on top. 
Assuming you are starting from scratch and don't have anything installed on your computer. 

## Installing Node.js and npm
One way is to visit the official [Node.js website](https://nodejs.org/) and download the latest version since it should be more stable.

Other way is via terminal 
```
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | sudo gpg --dearmor -o /usr/share/keyrings/nodesource-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/nodesource-archive-keyring.gpg] https://deb.nodesource.com/node_18.x $(lsb_release -s -c) main" | sudo tee /etc/apt/sources.list.d/nodesource.list > /dev/null
sudo apt update
sudo apt install nodejs
```

_Note: Node Package Manager (npm) is included with your Node.js installation._

Verify that `Node.js` & `npm` (Node Package Manager) have been installed

```
node -v
npm -v

Output
// v18.x.x
// v9.x.x
```

_Note: recommended node version is 18 or higher._

## Installing yarn package manager
Once Node.js and npm have been installed we can proceed with installing `yarn`
```
npm install -g yarn
```
_Note: `-g` flag means that `yarn` will be installed globaly on your system rather than locally in current project._

Verify that `yarn` has been installed
```
yarn -v

Output
// v1.22.x
```

## New Node project

Assuming you have created repository on your Github account.

Navigate to desired folder and clone the repository from Github via SSH (Secure Shell i.e. more secure than HTTPS), command should look like this
```
git clone git@github.com:[github-username]/[repository-name].git
```
Initiate a new Node.js project that will create `package.json` file

```
yarn init -y
```
_Note: flag `-y` means it will run  default installation._ 


## Playwright installation 
Now that we have `package.json` we can install the playwright framework.
```
yarn create playwright 
```
Command will initialize new project, and ask you questions regarding project, here are my answers.
```
✔ Do you want to use TypeScript or JavaScript? · TypeScript
✔ Where to put your end-to-end tests? · tests
✔ Add a GitHub Actions workflow? (y/N) · false
✔ Install Playwright browsers (can be done manually via 'yarn playwright install')? (Y/n) · false
✔ Install Playwright operating system dependencies (requires sudo / root - can be done manually via 'sudo yarn playwright install-deps')? (y/N) · false
```
After installation there should be some new folders in the repository root:
- node_modules
- tests
- tests-examples
- .gitignore
- playwright.config.ts 
- yarn.lock

Confirm that configuration is working as expected
```
npx playwright test
```
_Note: `npx` (Node Package Runner) comes with `npm`, used to execute Playwright test scripts._

Since we are using `yarn` we can modify scripts within our `package.json` 
```
"scripts": {
    "test": "npx playwright test"
}
``` 
Then execute tests using `yarn`
```
yarn test
``` 

## Synpress installation
Install the packages from the official [Synpress repository](https://github.com/Synthetixio/synpress) 
```
yarn add -D @synthetixio/synpress
```
Choose between [isolated-state](https://github.com/synpress-io/synpress-examples/tree/master/playwright/isolated-state), [shared-state](https://github.com/synpress-io/synpress-examples/tree/master/playwright/shared-state) or [isolated-state-with-eslint](https://github.com/synpress-io/synpress-examples/tree/master/playwright/eslint) example.

Here we are using `isolated-state`.

Next step is to create `fixtures.ts` file somewhere inside your test folder. Inside of it copy code from the [fixture.ts](https://github.com/synpress-io/synpress-examples/blob/master/playwright/isolated-state/fixtures.ts) on Synpress repository.

Within of our `example.test.ts` file we should import `{test, expect}` from `fixture.ts` insted of `@playwright/test`
```
// import { test, expect } from '@playwright/test';
import { test, expect } from '../tests/fixtures'
```

Inside `playwright.config.ts` changed config so the tests run in `headfull` mode
```
use: {
    headless: false,
    trace: 'on-first-retry',
},
```
Confirm that everything is working as expected 
```
npx playwright test
```
Playwright will execute 6 tests using 2 workers i.e. 2 tests for each browser in the config.

_Note: comment-out browsers that you don't want tests to be executed on._
