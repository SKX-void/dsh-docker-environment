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
  -t dsh-env:0.1.5 .

<!-- DOCKER_BUILDKIT=1 docker build -t dsh-env:0.1.4 . -->