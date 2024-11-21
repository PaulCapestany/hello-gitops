# hello-gitops

set up webhook for repo: https://github.com/PaulCapestany/hello-web/settings/hooks

oc create secret docker-registry quay-credentials \
  --docker-server=quay.io \
  --docker-username=<QUAY_USERNAME> \
  --docker-password=<QUAY_PASSWORD> \
  --docker-email=<YOUR_EMAIL> \
  --namespace=hello-web-namespace

oc secrets link pipeline quay-credentials --for=pull,mount -n hello-web-namespace

oc apply -f install-argocd-image-updater.yaml -n openshift-gitops

oc create secret generic argocd-image-updater-secret \
  --from-literal=argocd.token=<YOUR_GITHUB_PERSONAL_ACCESS_TOKEN_HERE> \
  -n openshift-gitops
