# 也來玩玩 skaffold 好了

## 事前準備工作

1. 安裝 kubectl
2. 安裝 kind
3. 安裝 skaffold
4. clone skaffold 專案

```
git clone https://github.com/GoogleContainerTools/skaffold
```

## 快速開箱

* 使用 skaffold 的 examples 範例
* 看起來好方便捏！我們也來試試 Java 唄！

## 實驗的專案

來源：

* https://start.spring.io/
* https://code.quarkus.io/


觀察一下問題

* maven/gradle 下載地獄
* openjdk image 低消 600 MB

# maven/gradle 下載地獄

## build cache

https://docs.docker.com/develop/develop-images/build_enhancements/

> Docker Build enhancements for 18.09 release introduces a much-needed overhaul of the build architecture. By integrating BuildKit, users should see an improvement on performance, storage management, feature functionality, and security.

```
$ DOCKER_BUILDKIT=1 docker build .
```
```json
{
  "debug" : true,
  "experimental" : true,
  "features": {"buildkit": true}
}
```

如果沒有效果，試著在 Dockerfile 第 1 行加上：

```
# syntax=docker/dockerfile:experimental
```

## 使用 custom build

如果不想開 BuildKit 的話，修改 building 的流程

https://skaffold.dev/docs/tutorials/custom-builder/

修改打包 image 流程，先在 host 編譯好，Dockerfile 只做 `COPY` 的動作。

PS. 這不適用編譯工具在 image 內的情境，例如 GraalVM。

# OpenJDK Runtime


## 常見的解法

1. jlink
2. native-image (GraalVM)
3. 面對它、接受它、處理它、放下它 (投䧏!?)

## 反思 containerization

1. 為了 docker 的容器化
2. 為了 kubernetes 的容器化


