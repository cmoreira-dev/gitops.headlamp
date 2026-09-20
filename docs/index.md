# gitops.headlamp

Deploys [Headlamp](https://headlamp.dev/), a web UI for browsing and
managing the homelab's Kubernetes (Talos) cluster, at
[headlamp.cmoreira.dev](https://headlamp.cmoreira.dev).

## What's here

- `helm/headlamp/` — a thin wrapper chart around the upstream
  `headlamp` chart (`kubernetes-sigs/headlamp`, pinned in `Chart.yaml`).
  `values.yaml` configures in-cluster auth (`config.inCluster: true`) and
  OIDC sign-in, sourced from an `ExternalSecret` (`headlamp-oidc`) rather
  than a plain Kubernetes `Secret`.
- `argocd/headlamp.yaml` — the ArgoCD `Application` that reconciles this
  chart into the `headlamp` namespace, with automated sync, pruning, and
  self-heal enabled.
- `terraform/` — reserved for this repo's Tier 2 IaC (Burrito), currently
  empty.

## How it's deployed

ArgoCD watches `main` and applies `helm/headlamp` directly — there's no
CI build for this repo (it doesn't produce a container image, just Helm
values on top of the upstream chart). Pushing a change to `helm/headlamp`
is picked up by ArgoCD's automated sync.

## Related

- Depends on [`gitops.core-addons`](https://github.com/cmoreira-dev/gitops.core-addons)
  for cluster-wide addons (external-secrets, NGINX Gateway Fabric, cloudflared).
- Upstream chart: [kubernetes-sigs/headlamp](https://github.com/kubernetes-sigs/headlamp).
