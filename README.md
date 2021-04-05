
# Try locally

```sh
# angular does not support webpack 5 by default
# so we need to use yarn for building
ng config -g cli.packageManager yarn

# install dependencies
yarn

# build app
npm run build-prod

# start shell and mfe1, then open in browser at diff ports
npm run start
```

We should be capable to open `localhost:5000` and see

![](images/README/2021-04-05-02-07-52.png)

clicking on flights should redirect us to `mfe1`

![](images/README/2021-04-05-02-08-05.png)

and bokings is not implemented

![](images/README/2021-04-05-02-08-47.png)

# Azure

At first lets create Static Web App for `shell` of our microfrontends (`mfe`).

Create new Static Web App

![](images/README/2021-04-05-01-52-32.png)

Connect to github

![](images/README/2021-04-05-01-52-42.png)

As we have web app and api in root folder

![](images/README/2021-04-05-01-57-02.png)

we point app location to `/app` and api to `/api`. Output location is `dist/shell`:

![](images/README/2021-04-05-01-58-50.png)

Then we will do the same for `mfe1` microftontend.

After pulling from github we should have two new yamls:

![](images/README/2021-04-05-02-00-30.png)

Edit them one by one to be like. First for shell:

```yaml
name: Azure Static Web Apps CI/CD - SHELL

on:
  push:
    branches:
      - main
    paths:
      - app/projects/shell
      - app/projects/ui-lib
      - app/projects/message-bus
      - app/package.json
      - app/tsconfig.json
      - app/tslint.json
      - .github/workflows/azure-static-web-apps-lively-flower-04e21dc10.yml

  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main
    paths:
      - app/projects/shell
      - app/projects/ui-lib
      - app/projects/message-bus
      - app/package.json
      - app/tsconfig.json
      - app/tslint.json
      - .github/workflows/azure-static-web-apps-lively-flower-04e21dc10.yml
      
jobs:AZURE_STATIC_WEB_APPS_API_TOKEN_LIVELY_FLOWER_04E21DC10
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_LIVELY_FLOWER_04E21DC10 }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for Github integrations (i.e. PR comments)
          action: "upload"
          ###### Repository/Build Configurations - These values can be configured to match your app requirements. ######
          # For more information regarding Static Web App workflow configurations, please visit: https://aka.ms/swaworkflowconfig
          app_location: "/app" # App source code path
          api_location: "/api" # Api source code path - optional
          output_location: "dist/shell" # Built app content directory - optional

          app_build_command: npm run build-prod:shell
          ###### End of Repository/Build Configurations ######

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v0.0.1-preview
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_LIVELY_FLOWER_04E21DC10 }}
          action: "close"
```

Here used by github, deployment token `AZURE_STATIC_WEB_APPS_API_TOKEN_LIVELY_FLOWER_04E21DC10` can be found by clicking `Manage deployment token`:

![](images/README/2021-04-05-02-02-15.png)

After pushing changes to github, we can go to actions and verify successful build:

![](images/README/2021-04-05-02-12-35.png)

We can see that it take up to 5min. 

![](images/README/2021-04-05-02-20-27.png)

Then we can open [Shell](https://lively-flower-04e21dc10.azurestaticapps.net/) and [Mfe1](https://salmon-moss-0f41c3910.azurestaticapps.net) in the browser. Our pages should be served the same as in local development. 

Azure is using highly efficient CDN servers to serve our static files.



# Development

```sh
# angular does not support webpack 5 by default
# so we need to use yarn for building
ng config -g cli.packageManager yarn

# install dependencies
yarn

# concurrent build with watching and rebuilding on failures, plus concurent start
npm run dev
```


## Github

### Init root with submodules

```sh

( # subshell to not keep trash in bash
    set -u
    set -o pipefail

    ROOT_REPO='git@github.com:jon-grey/sample-app-module-federation-azure-web-app-angular-azure-function-api-python.git'
    APP_REPO='git@github.com:jon-grey/sample-app-module-federation-azure-web-app-angular-azure.git'
    API_REPO='git@github.com:jon-grey/sample-app-module-federation-azure-function-api-python.git'

    (
        echo "[INFO] setting up APP subrepo: ${APP_REPO}"
        cd app
        git init
        git add .
        git commit -m "first commit"
        git branch -M main
        git remote add origin ${APP_REPO}
        git push -u origin main
    )
    (    
        echo "[INFO] setting up API subrepo: ${API_REPO}"
        cd api
        git init
        git add .
        git commit -m "first commit" 
        git branch -M main
        git remote add origin ${API_REPO}
        git push -u origin main
    )
    (        
        echo "[INFO] setting up ROOT subrepo: ${ROOT_REPO}"
        git init
        echo "[INFO] setting up ROOT submodule APP: ${APP_REPO}"
        git submodule add ${APP_REPO} app 
        echo "[INFO] setting up ROOT submodule API: ${API_REPO}"
        git submodule add ${API_REPO} api 
        git add .
        git commit -m "first commit" 
        git branch -M main
        git remote add origin ${ROOT_REPO}
        git push -u origin main
    )
)

```


### Update github repos

```sh

## Update root
(
    set -e
    set -u
    set -o pipefail

    MSG="Updated readme"

    echo "[INFO] updating up ROOT repo:"
    git add .
    git commit -m "${MSG}"
    git push -u origin main
)
## Update submodule app
(
    set -e
    set -u
    set -o pipefail
    SUB="app"
    MSG="Updated ${SUB}"
    MSG="Make local libs as dependencies in package.json - build with apps."

    (
        echo "[INFO] updating up ${SUB} subrepo:"
        cd ${SUB}
        git add .
        git commit -m "${MSG}"
        git push -u origin main
    )
    (
        echo "[INFO] updating up ROOT repo:"
        git add ${SUB}
        git commit -m "Updated submodule ${SUB}"
        git push -u origin main
    )
)
## Update submodule api
(
    set -e
    set -u
    set -o pipefail    
    SUB="api"
    MSG="Updated ${SUB}"

    (
        echo "[INFO] updating up ${SUB} subrepo:"
        cd ${SUB}
        git add .
        git commit -m "${MSG}"
        git push -u origin main
    )
    (
        echo "[INFO] updating up ROOT repo:"
        git add ${SUB}
        git commit -m "Updated submodule ${SUB}"
        git push -u origin main
    )
)

```

### Useful(less) commands

```sh

# can not add submodule -> ??? already in the index
git rm -r --cached --force app
git rm -r --cached --force api

```
