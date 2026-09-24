<!-- docker buildx build --load --platform linux/amd64 -t dsh-env:0.1.4 .
docker buildx build --load --platform linux/amd64,linux/arm64 -t dsh-env:0.1.4 .

docker buildx build --load --platform linux/arm64 -t dsh-env:0.1.4 .
docker save -o dsh-env.tar dsh-env:0.1.4 -->

<!-- 配代理最头疼了 -->
export HTTP_PROXY=http://127.0.0.1:7890
export HTTPS_PROXY=http://127.0.0.1:7890
export NO_PROXY=localhost,127.0.0.1
docker build \
  --build-arg http_proxy="http://172.17.0.1:7890" \
  --build-arg https_proxy="http://172.17.0.1:7890" \
  --build-arg ftp_proxy="http://172.17.0.1:7890" \
  --build-arg no_proxy="localhost,127.0.0.1,::1" \
  -t dsh-env:0.2.0 .

<!-- DOCKER_BUILDKIT=1 docker build -t dsh-env:0.1.4 . -->


```bash

# export HTTP_PROXY=http://127.0.0.1:7890
# export HTTPS_PROXY=http://127.0.0.1:7890
# export NO_PROXY=localhost,127.0.0.1

docker compose -f docker-compose.fix.yml up -d
docker exec -it dsh-fix bash

export HTTP_PROXY=http://172.11.0.1:7890
export HTTPS_PROXY=http://172.11.0.1:7890
export NO_PROXY=localhost,172.11.0.1

git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
git checkout tags/dsh-v0.1.5-rc.1

pnpm install
pnpm run build
pnpm dsh web

# ---

docker exec -it dsh-fix bash


export HTTP_PROXY=http://172.11.0.1:7890
export HTTPS_PROXY=http://172.11.0.1:7890
export NO_PROXY=localhost,172.11.0.1

cd /app
git clone https://github.com/zhu1090093659/dsh-web.git
cd dsh-web
git checkout tags/v0.3.24
# 2. 安装依赖并构建
pnpm install
pnpm -r build

node scripts/link-profile.mjs

cd /app/deepseek-harness
pnpm dsh plugin --profile web add link:/app/dsh-web/packages/dsh-web-all
```


升级，0.1.7与0.4.2的代码有问题，回滚
```bash
docker exec -it dsh-fix bash

cd /app/deepseek-harness
# 暂存源代码改动
git stash push packages/sandbox/sandbox-local/src/profiles.ts
git pull
git checkout tags/dsh-v0.1.5-rc.3
# 这里不清理构建产物会导致构建失败
git clean -Xdf -e node_modules -e .pnpm-store
pnpm install --frozen-lockfile
pnpm build
git stash pop stash@{0}

cd /app/dsh-web
git pull
git checkout tags/v0.3.24
# git clean -Xdf -e node_modules -e .pnpm-store
# pnpm install --frozen-lockfile
pnpm install
pnpm -r build
node scripts/link-profile.mjs
```