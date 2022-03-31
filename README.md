# terragrunt-experiment-manifests
ArgoCD Manifests for the experiment. Features:

- Nginx Ingress controller
- Cert Manager
- [Shipwright](https://shipwright.io/)
  + Includes a BuildStrategy [that I wrote](https://github.com/shipwright-io/build/blob/main/samples/buildstrategy/kaniko/buildstrategy_kaniko-trivy_cr.yaml)
- [ArgoCD](https://argoproj.github.io/cd/)
- [imago](https://github.com/philpep/imago) to refresh builds
- Argo Events is included but not yet used


## What's Hard Coded?

- The `AppProject` resources in `projects/` contain git repo urls:
  + under `.spec.sourceRepos` for RBAC

- The `ApplicationSet` resources in `projects/` also have hard-coded repo urls
  + under `.spec.template.spec.source.repoURL`
  
- Cert Manager (`applications/platform/cert-manager`) has an email address for the ClusterIssuer 

- The (undeployed) Argo Events `EventSource` repo has a url, which would point to an application repo (i.e. `demo-app`)

- The Ingress Controller `Application` resource (which deploys a helm chart) has the domain name baked into an annotation (`service.beta.kubernetes.io/do-loadbalancer-name: example-demo.com`). This is for configuring the cloud resources.

- The `web-system` namespace has both git repos and docker registries
  + The git repos references the images to be built
  + The registry url is the destination to push the image to
