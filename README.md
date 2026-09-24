# GeeUIComponents

Shared library for the other robot apps. Network calls are built in `GeeUINetworkUtil`.

The host `https://yourservice.com` is a string literal, not a constant. Paths are the fields of `GeeUINetworkConsts`. The final URL is `https://yourservice.com` + `uri` + `?sn&ts`. Details, including which paths are still the placeholder `your interface url`, are in [docs/NETWORK.md](docs/NETWORK.md).
