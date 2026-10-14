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
```bash
docker build -t dsh-env:0.4.0 .

```
```bash

export HTTP_PROXY=http://127.0.0.1:7890
export HTTPS_PROXY=http://127.0.0.1:7890
export NO_PROXY=localhost,127.0.0.1
docker compose -f docker-compose.fix.yml down && docker compose -f docker-compose.fix.yml up -d
docker exec -it dsh-fix bash

export HTTP_PROXY=http://172.11.0.1:7890
export HTTPS_PROXY=http://172.11.0.1:7890
export NO_PROXY=localhost,172.11.0.1

git clone https://github.com/deepseek-ai/deepseek-harness.git
cd deepseek-harness
git checkout tags/dsh-v0.2.0-rc.2

pnpm install
pnpm run build
pnpm dsh web

# ---

docker exec -it dsh-fix bash


export HTTP_PROXY=http://172.20.2.1:7890
export HTTPS_PROXY=http://172.20.2.1:7890
export NO_PROXY=localhost,172.20.2.1

cd /app
git clone --filter=blob:none https://github.com/zhu1090093659/dsh-web.git
cd dsh-web
git checkout tags/v0.3.24
# 2. 安装依赖并构建
pnpm install
pnpm -r build

node scripts/link-profile.mjs

cd /app/deepseek-harness
pnpm dsh plugin --profile web add link:/app/dsh-web/packages/dsh-web-all


# cd /app
# git clone https://github.com/Tyan66666/billion-context-dsh.git
# cd billion-context-dsh
# git checkout tags/v0.2.26
# npm install
# npm run build

cd /app/deepseek-harness
pnpm dsh plugin --profile web add link:/app/billion-context-dsh
```
sudo mkdir -p /mnt/usbssd
sudo mount /dev/sdc1 /mnt/usbssd

升级，0.1.7与0.4.2的代码有问题，回滚
```bash
docker exec -it dsh-fix bash

cd /app/deepseek-harness
# 暂存源代码改动
# git stash push packages/sandbox/sandbox-local/src/profiles.ts
git pull
git checkout tags/dsh-v0.2.0-rc.2

# 这里不清理构建产物会导致构建失败
git clean -Xdf -e node_modules -e .pnpm-store
pnpm install --frozen-lockfile
pnpm build
git stash pop stash@{0}

cd /app/dsh-web
git pull
git checkout tags/v0.4.4
# git clean -Xdf -e node_modules -e .pnpm-store
# pnpm install --frozen-lockfile
pnpm install
pnpm -r build
node scripts/link-profile.mjs



# 修复原版源代码landlock-run二进制缺失无法对接沙箱的问题
cd /tmp
npm pack @deepseek-ai/node-addon-system-linux-x64@0.1.2
tar -xzf deepseek-ai-node-addon-system-linux-x64-0.1.2.tgz
mkdir -p /app/deepseek-harness/native/system/packages/linux-x64/bin
cp package/bin/landlock-run /app/deepseek-harness/native/system/packages/linux-x64/bin/landlock-run
chmod 755 /app/deepseek-harness/native/system/packages/linux-x64/bin/landlock-run
/app/deepseek-harness/native/system/packages/linux-x64/bin/landlock-run --probe   # 期望输出 full
```
