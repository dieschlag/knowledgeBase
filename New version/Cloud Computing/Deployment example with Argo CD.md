The principle of **ArgoCD** is to automate the deployment of applications on **Kubernetes**. We need a **Git repository** that contains all the configurations, along with a way to automatically apply them.

**Helm** groups all the manifests of an application into a **Chart**. It's a folder that might look like this:

``` yaml
├── **templates** -> Dossier qui contient tous tes manifestes
│   ├── _certificate.yaml_
│   ├── _deployment.yaml_
│   ├── _ingress.yaml_
│   └── _service.yaml_
├── Chart.yaml -> Contient les infos principales de ta charte:nom, version,etc...
└── values.yaml -> variables utilisées dans les autres fichiers
```

The strength of Helm charts lies in **templating**, meaning you can use variables.

For example, if you have the following in `values.yaml`:

```
foo:
   bar: coucou
   via:
     rezo: supelec
```

Then you can access it, for instance, by using `{{ .Values.foo.bar }}`.

**ArgoCD** enables **GitOps**, meaning we use **Git to deploy applications**. This way, we no longer need to run scripts manually.

TO COMPLETE

