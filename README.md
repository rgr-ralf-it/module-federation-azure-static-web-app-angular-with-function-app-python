

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


## Update root

```sh
(
    set -e
    set -u
    set -o pipefail

    SUBM="app"
    MSG="."

    echo "[INFO] updating up ROOT repo:"
    git add .
    git commit -m "${MSG}"
    git push -u origin main
)

```

## Update submodules

```sh
(
    set -u
    set -o pipefail
    
    (
        set -e
    
        SUBM="app"
        MSG="."

        (
            echo "[INFO] updating up APP subrepo:"
            cd ${SUBM}
            git add .
            git commit -m "${MSG}"
            git push -u origin main
        )
        (
            echo "[INFO] updating up ROOT repo:"
            git add ${SUBM}
            git commit -m "Updated submodule ${SUBM}"
            git push -u origin main
        )
    )
    (
        set -e
    
        SUBM="api"
        MSG="."

        (
            echo "[INFO] updating up APP subrepo:"
            cd ${SUBM}
            git add .
            git commit -m "${MSG}"
            git push -u origin main
        )
        (
            echo "[INFO] updating up ROOT repo:"
            git add ${SUBM}
            git commit -m "Updated submodule ${SUBM}"
            git push -u origin main
        )
    )
)


```

## Usefull commands

```sh
# 
git push --recurse-submodules

# can not add submodule -> ??? already in the index
git rm -r --cached --force app
git rm -r --cached --force api

```
