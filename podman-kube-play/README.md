# Podman Play with Kube: Devconf.cz 2025

Original version by Mario Loriedo, found [here](https://github.com/l0rd/talks/tree/gh-pages/podman-kube-play)

![slide](slide.png)

### Running

To run the presentation:

```bash
presenterm ./slides.md
```

### Prerequisites

* `podman.socket` must be started for the user running the demo
* You should pull the `redis` and `alpine` images before the demo
* `docker compose` must be installed in a path that Podmna recognizes, such that the `podman compose up` command works when run in this directory. For best results, one it at least once before starting the slides, so the images are pre-built.

### Presenterm Config

To allow snipped execution:

```bash
cat <<EOF >  ~/.config/presenterm/config.yaml
snippet:
  exec:
    enable: true
  exec_replace:
    enable: true
EOF
```

- [Presenterm documentation](https://mfontanini.github.io/presenterm/introduction.html)
- [Presenterm config](https://github.com/mfontanini/presenterm/blob/master/config.sample.yaml)
