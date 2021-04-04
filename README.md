

# Github

## Init root with submodules

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


## Update github repos

```sh

## Update root
(
    set -e
    set -u
    set -o pipefail

    MSG="Updated root"

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

## Useful(less) commands

```sh

# can not add submodule -> ??? already in the index
git rm -r --cached --force app
git rm -r --cached --force api

```
