## Steps to patch next-swc for linux

1. Login to `ghcr.io`

2. Build below Dockerfile using command:
```
$ docker build -t next-swc-linux-image .
```

Dockerfile:
```
FROM ghcr.io/napi-rs/napi-rs/nodejs-rust:stable-2023-09-17-x64

WORKDIR /next

RUN set -e &&
  apt update &&
  apt install -y pkg-config xz-utils dav1d libdav1d-dev &&
  rustup toolchain install nightly-2023-11-16 &&
  rustup default nightly-2023-11-16 &&
  rustup target add x86_64-unknown-linux-gnu &&
  npm i -g "@napi-rs/cli@2.16.2" &&
  unset CC_x86_64_unknown_linux_gnu && unset CC
```

3. Run docker image created above by running following command (in root of next repo):
```
$ docker run -it --mount type=bind,source="$(pwd)",target=/next next-swc-linux-image:latest bash
```

4. Run followin commands inside container:
```
$ cd packages/next-swc
$ npm run build-native-release
$ strip native/next-swc.*.node
```