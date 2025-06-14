---
theme:
  name: catppuccin-latte
title: Play with Kube using Podman
# sub_title: Podman - The Next Generation Container Engine
author: Mario Loriedo & Matt Heon
---

Agenda
---

### Podman
### Podman Pod
### Kubernetes Pod
### `podman kube play`
### `podman kube generate`
### Supported Kubernetes Objects
### More Awesomeness
### Resources

<!-- end_slide -->

Podman
---

<!-- column_layout: [1, 2] -->

<!-- column: 0 -->

## First release in 2018

<!-- pause -->

#### Runs containers on Linux (native), Windows, Mac (VMs)

<!-- pause -->

##### Recently accepted as a CNCF Project

<!-- pause -->

###### Current version 5.5.1

<!-- column: 1 -->

<!-- pause -->
```bash +exec
./clean
```

<!-- pause -->
```bash +exec
podman run -d -p 8080:80 httpd
```

<!-- pause -->
```bash +exec
curl -s localhost:8080
```

<!-- end_slide -->

Why Kubernetes YAML?
---

## This was a Podman 1.0 feature - around 6 years old!
<!-- pause -->
## At the time, Podman did not support the Docker API - or Docker Compose
<!-- pause -->
## We wanted a proper alternative.
<!-- pause -->
## Kubernetes YAML is common, and we can do many, many things with it.


<!-- end_slide -->

Podman Pods
---

<!-- column_layout: [1, 2] -->

<!-- column: 0 -->

```bash +exec
./clean
```

<!-- pause -->

```bash +exec
podman pod create -p 8080:8080 --name myPod
```

<!-- pause -->

```bash +exec
podman create --pod myPod --name redis --read-only -v webapp_redis:/data redis:alpine redis-server --appendonly yes --notify-keyspace-events Ex
podman create --pod myPod --name webapp --read-only localhost/hello-py-aioweb:latest
```

<!-- pause -->

<!-- column: 1 -->

```bash +exec
podman pod start myPod
```

<!-- pause -->

```bash +exec
podman ps --format '{{ .ID }} {{ .PodName }} {{ .State }}'
```

<!-- pause -->

```bash +exec
podman pod stop myPod
podman pod rm myPod
```

<!-- end_slide -->

Kubernetes Pods
---
```file {1-30|10-18|19-25|26-29} +line_numbers
path: pod.yaml
language: yaml
```
<!-- end_slide -->

<!-- jump_to_middle -->

`podman kube play`
---

<!-- end_slide -->

`podman kube play`
---

<!-- column_layout: [1, 1] -->

<!-- column: 0 -->

```bash +exec
./clean
```
<!-- pause -->

```bash +exec
podman kube play ./pod.yaml
```
<!-- pause -->

```bash +exec
podman ps --format "{{.Image}}"
```
<!-- pause -->

<!-- column: 1 -->
```bash +exec
podman volume ls
```
<!-- pause -->

```bash +exec
curl -s localhost:8080
```

<!-- pause -->

```bash +exec
curl -s localhost:8080
```

<!-- end_slide -->

`podman kube play --build`
---

```bash +exec
./clean
```
<!-- pause -->

```bash +exec
podman rmi localhost/hello-py-aioweb:latest
```

<!-- end_slide -->

`podman kube play --build`
---

```bash +exec
podman kube play ./pod.yaml --build
```

<!-- end_slide -->

`podman kube play --build`
---
<!-- column_layout: [1, 2] -->

<!-- column: 0 -->
```bash +exec
podman images localhost/hello-py-aioweb:latest
```
<!-- pause -->

```bash +exec
podman ps --format "{{.Image}}"
```
<!-- pause -->

<!-- column: 1 -->
```bash +exec
podman volume ls
```
<!-- pause -->

```bash +exec
curl -s localhost:8080
```

<!-- end_slide -->

`podman kube generate`
---

```bash +exec
./clean
```
<!-- pause -->

```bash +exec
podman run -d -p 8080:80 httpd
```
<!-- pause -->

```bash +exec
podman ps --format "{{.Image}}"
```

<!-- end_slide -->

`podman kube generate`
---

```bash +exec
ID=$(podman ps --last 1 -q)
podman kube generate ${ID}
```

<!-- end_slide -->

Generating things not in Kubernetes
---
<!-- column_layout: [1, 2] -->

<!-- column: 0 -->
```bash +exec
./clean

podman create --ulimit nofile=1024:1024 --name testctr alpine sh
podman generate kube testctr
```

<!-- column: 1 -->
<!-- pause -->
```bash +exec
podman generate kube testctr > testctr.yaml
podman rm -af
podman play kube testctr.yaml
rm -f testctr.yaml
```

<!-- pause -->
```bash +exec
podman inspect --latest --format '{{ .HostConfig.Ulimits }}'
```

<!-- end_slide -->

Migrating from Compose
---
<!-- column_layout: [1, 2] -->

<!-- column: 0 -->
```bash +exec
./clean
```

<!-- pause -->

```bash +exec
cat ./compose.yaml
```

<!-- pause -->

```bash +exec
podman compose up --detach
```

<!-- pause -->

<!-- column: 1 -->

```bash +exec
podman generate kube podman-kube-play-web-1
```

<!-- end_slide -->

Migrating from Compose, Part 2
---

<!-- column_layout: [1, 2] -->

<!-- column: 0 -->
```bash +exec
./clean
podman compose up --detach
echo -----
podman ps --format '{{ .ID }} {{ .Names }} ({{ .PodName }}) '
```

<!-- pause -->

```bash +exec
curl -s 127.0.0.1:8080
```

<!-- pause -->

<!-- column: 1 -->
```bash +exec
podman generate kube podman-kube-play-web-1 > testcompose.yaml
podman rm -af
podman play kube testcompose.yaml
echo -----
podman ps --format '{{ .ID }} {{ .Names }} ({{ .PodName }}) '
```

<!-- pause -->

```bash +exec
curl -s 127.0.0.1:8080
```


<!-- end_slide -->

Supported Kubernetes Objects
---

## Pods
<!-- pause -->
## Deployments
<!-- pause -->
## PersistentVolumeClaims
<!-- pause -->
## ConfigMaps
<!-- pause -->
## Secrets
<!-- pause -->
## DaemonSets
<!-- pause -->
## Jobs

<!-- end_slide -->

More (Podman) Awesomeness
---

<!-- pause -->
#### Rootless and Daemonless
<!-- pause -->
#### Build Farms
<!-- pause -->
#### Image Volumes
<!-- pause -->
#### Quadlet
<!-- pause -->
#### Podmansh

<!-- end_slide -->

Resources To Learn More
---

### [podman.io](https://podman.io)
### [github.com/containers/](https://github.com/container/)
### [presenterm](https://github.com/mfontanini/presenterm)
### [this deck](https://github.com/l0rd/talks/tree/gh-pages/podman-kube-play)

<!-- end_slide -->
