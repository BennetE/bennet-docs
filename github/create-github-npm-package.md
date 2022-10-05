# Create a private npm package hosted on GitHub

- Step 1: Create a package hosted on GitHub
- Step 2: Use (install) the package in another project
- Step 3: Use (install) the package in a GitHub Action workflow

## Step 1: Create a package hosted on GitHub

1. Create a new private GitHub repository
2. Create `package.json` with the following content:
    - Replace `YOUR_GITHUB_USERNAME` with your GitHub username or organization name
    - Replace `YOUR_PACKAGE_NAME` with the name of **your repository**

```json
{
  "name": "@YOUR_GITHUB_USERNAME/YOUR_PACKAGE_NAME",
  "version": "1.0.0",
  "publishConfig": {
    "registry": "https://npm.pkg.github.com/@YOUR_GITHUB_USERNAME"
  }
}
```

3. Create `index.js` with the following content:

 ```javascript
const sayHello = function (name) {
    console.log('Hello ' + name + '!');
};

module.exports = {
    sayHello
};
```

4. Create GitHub actions workflow:
    - Select the `Actions` tab in your repository
    - Click `New workflow`
    - Search for `npm-publish-github-packages` and select `configure`
    - commit the changes (to create the workflow)
    - Alternatively, is here the content of the workflow file:

```yaml
# This workflow will run tests using node and then publish a package to GitHub Packages when a release is created
# For more information see: https://help.github.com/actions/language-and-framework-guides/publishing-nodejs-packages

name: Node.js Package

on:
  release:
    types: [ created ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
      - run: npm ci
      - run: npm test

  publish-gpr:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          registry-url: https://npm.pkg.github.com/
      - run: npm ci
      - run: npm publish
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
```

5. Create a new release to trigger the workflow and publish the package
6. Configure package access:
   - On the repository start page, select the package under `Packages` tab on the right
   - Select `Package Settings` on the right
   - Select `Add Repository` under `Manage Actions access`
     - Select the repository where you (the GitHub actions) want to use the package

## Step 2: Use (install) the package in another project

1. Create a new (private) GitHub repository
2. Generate a new `package.json` file with `npm init -y`
3. Get a personal access token from GitHub
    - Go to `Settings` tab on GitHub.com
    - Select `Developer settings` in the left menu
    - Select `Personal access tokens` in the left menu
    - Click `Generate new token`
    - Set a useful note to remember what the token is for
    - Select expiration date (e.g. no expiration)
    - Select `read:packages` and click `Generate token`
    - Copy the generated token for the next step
    - (Private access tokens can only be created with a personal GitHub account, but can then be used in an
      organization)
4. Create a `.npmrc` file with the following content:
    - Replace `YOUR_GITHUB_USERNAME` with your GitHub username or organization name where the package is hosted
    - Replace `YOUR_PERSONAL_ACCESS_TOKEN` with the personal access token you generated in the previous step

```
@YOUR_GITHUB_USERNAME:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=YOUR_PERSONAL_ACCESS_TOKEN
```

5. Install the package with `npm install @YOUR_GITHUB_USERNAME/YOUR_PACKAGE_NAME`
6. Add the `.npmrc` file to `.gitignore` to prevent it from being committed to the repository

## Step 3: Use (install) the package in a GitHub Action workflow

1. Create a new GitHub Actions workflow
2. Add the following workflow `.github/workflows/test.yml`:
   - Replace `YOUR_GITHUB_USERNAME` with your GitHub username or organization name where the package is hosted

```yaml
name: Access GitHub Packages in a workflow

on:
  push:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: 16
          always-auth: true
          registry-url: https://npm.pkg.github.com/@YOUR_GITHUB_USERNAME
        env:
          NODE_AUTH_TOKEN: ${{secrets.GITHUB_TOKEN}}
      - run: npm i
      - run: npm start
```

3. With this workflow, the package will be installed from GitHub Packages
