```sh
flux bootstrap github --owner=FelixDobler --repository=kubernetes --branch=main --path=./clusters/production --personal --token-auth --components-extra=source-watcher
```

Maybe switch to export so the commit can be manually signed.
```sh
flux install --components-extra=source-watcher --export > ./clusters/production/flux-system/gotk-components.yaml
```