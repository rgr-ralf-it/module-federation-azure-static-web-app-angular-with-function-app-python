

# Setup github



```sh

( # subshell to not keep trash in bash
    set -u
    set -o pipefail

    ROOT_REPO='git@github.com:jon-grey/sample-app-module-federation-azure-web-app-angular-azure-function-api-python.git'
    APP_REPO='git@github.com:jon-grey/sample-app-module-federation-azure-web-app-angular-azure.git'
    API_REPO='git@github.com:jon-grey/sample-app-module-federation-azure-function-api-python.git'

    (
        set -e

        echo "[INFO] setting up APP subrepo: ${APP_REPO}"
        cd app
        git init
        git add .
        git commit -m "first commit" || true
        git branch -M main
        git remote add origin ${APP_REPO} || true
        git push -u origin main
    )

    (
        set -e
    
        echo "[INFO] setting up API subrepo: ${API_REPO}"
        cd api
        git init
        git add .
        git commit -m "first commit" || true
        git branch -M main
        git remote add origin ${API_REPO} || true
        git push -u origin main
    )

    (
        set -e
        
        echo "[INFO] setting up root subrepo: ${ROOT_REPO}"
        git init
        echo "[INFO] setting up root submodule APP: ${APP_REPO}"
        git submodule add ${APP_REPO} api || true
        echo "[INFO] setting up root submodule API: ${API_REPO}"
        git submodule add ${API_REPO} app || true
        git add .
        git commit -m "first commit" || true
        git branch -M main
        git remote add origin ${ROOT_REPO} || true
        git push -u origin main
    )
)

```
