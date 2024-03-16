# fluxcdpublic
basic setup of flux and helmrelease

## prepare
```
export GITHUB_TOKEN=<your-token>
export GITHUB_USER=<your-username>

curl -s https://fluxcd.io/install.sh | sudo bash
```

## check
```
flux check --pre
```

## install
```
flux bootstrap github \
  --owner=$GITHUB_USER \
  --repository=fluxcd \
  --branch=main \
  --path=home \
  --personal
```

## add gitrepro
```
flux create source git testapp --url=https://github.com/wlanboy/argocd --branch=main
flux export source git testapp > gitressource-testapp.yaml
flux delete source git testapp
```

## add helmrelease
```
flux create helmrelease testapp --interval=10m --source=GitRepository/testapp --chart=./test --create-target-namespace --target-namespace test
flux export helmrelease testapp > helmrelease-testapp.yaml
flux delete helmrelease testapp
```

## lookup events
```
flux events
kubectl get all -n test
flux get all
```

## update git source
```
git add gitressource-testapp.yaml helmrelease-testapp.yaml
git commit -m "added testapp git repro and helm release" && git push
flux reconcile source git flux-system
```
