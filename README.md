# PlatformCon-2024

### Helm chart installtion steps

Follow quickstart [guide](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller/quickstart-for-actions-runner-controller)

1. Install Controller
```sh
NAMESPACE="arc-systems"
helm install arc \
    --namespace "${NAMESPACE}" \
    --create-namespace \
    oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set-controller
```

2. Configure Github App or PAT for [authentication](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners-with-actions-runner-controller/authenticating-to-the-github-api#deploying-using-personal-access-token-classic-authentication)

3. Install runner scale set

```sh
INSTALLATION_NAME="arc-runner-set"
NAMESPACE="arc-runners"
GITHUB_CONFIG_URL="https://github.com/GitKaran/PlatformCon-2024"
GITHUB_PAT="<YOUR-GITHUB-PAT>"
helm install "${INSTALLATION_NAME}" \
    --namespace "${NAMESPACE}" \
    --create-namespace \
    --set githubConfigUrl="${GITHUB_CONFIG_URL}" \
    --set githubConfigSecret.github_token="${GITHUB_PAT}" \
    oci://ghcr.io/actions/actions-runner-controller-charts/gha-runner-scale-set
```

4. Check installed charts
```sh
~ ❯ helm list -A | grep gha-runner                                                                                                                                                                         ⎈ minikube

arc           	arc-systems 	1       	2024-04-15 00:21:13.268985 +0200 CEST	deployed	gha-runner-scale-set-controller-0.9.0	0.9.0
arc-runner-set	arc-runners 	1       	2024-04-15 00:21:43.728409 +0200 CEST	deployed	gha-runner-scale-set-0.9.0           	0.9.0
~ ❯
```

5. Check pods
   
```sh
~ ❯ kubectl get pods -n arc-systems                                                                                                                                                                        ⎈ minikube
NAME                                     READY   STATUS    RESTARTS   AGE
arc-gha-rs-controller-869bf56bc8-7w8lk   1/1     Running   0          6m36s
arc-runner-set-754b578d-listener         1/1     Running   0          151m
~ ❯
   
```
