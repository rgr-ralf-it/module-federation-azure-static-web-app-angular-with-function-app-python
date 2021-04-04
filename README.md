

# Setup github



```sh

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
echo "[INFO] setting up root subrepo: ${ROOT_REPO}"
git init
echo "[INFO] setting up root submodule APP: ${APP_REPO}"
git submodule add ${APP_REPO} api
echo "[INFO] setting up root submodule API: ${API_REPO}"
git submodule add ${API_REPO} app
git add .
git commit -m "first commit"
git branch -M main
git remote add origin ${ROOT_REPO}
git push -u origin main
)

```
