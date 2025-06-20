Première étape: utiliser un Deployement, il permet de générer plusieurs répliques d'un même controlleur et ainsi regouper ensemble, avec la même configuration plusieurs conteneurs et enfin, gérer la mise à jour vers une nouvelle version.

Exemple d'un Deployement:

``` yaml
 apiVersion: apps/v1
kind: Deployment
metadata:
  name: nom_de_ton_deploiement
  labels:
    app: nom_de_ton_app
spec:
  replicas: 3 #Nombre de pods qui vont être déployés
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx #Nom du conteneur
          image: nginx:latest #Image que l'on va utiliser
          ports:
            - containerPort: 80 #port exposé par l'image
```

Pour appliquer le fichier: 

```
kubectl apply -f nom_de_ta_charte.yml
```

Pour afficher les Deployment lancés:

```
kubectl get deployment
```

Et pour voir les pods:

```
kubectl get pods
```

Puis pour rendre accessible les ports qui ont été lancés, on fait du port forwarding pour mapper un port du localhost avec un port d'un pod, avec la commande:

```
kubectl port-forward pods/<nom> <port-local>:<port-conteneur>
```

Deuxième étape: rendre accessible son site à l'extérieur du cluster

Premier problème actuellement: nos pods vont changer de nom à chaque redémarrage, il faut donc créer un Service pour que tous les pods soient accessible sous un nom/port unique depuis l'intérieur du cluster

Exemple de Service:

``` yaml
apiVersion: v1
kind: Service
metadata:
  name: nom_de_ton_service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

Point important: le selector va cibler tous les pods qui le lable `nom_de_service` dans le champ app qui est déclaré dans le champ du `Deployement`. Il faut donc faire attention aux noms qui sont donnés.

Le port est ici le port du host que l'on lit au targetPort des pods liés au selector choisi.

A présent il faut générer une route pour pouvoir accéder depuis l'extérieur et utiliser un Certificate pour avoir de l'HTTPS.

La route s'obtient avec un Ingress Route. Un Ingress désigne une ressource native à Kubernetes, ici IngressRoute est un ajout du plugin Traefik, qui permet une gestion de plus haut niveau du réseau Kubernetes.

Traefix est un reverse proxy (= type de proxy qui se place devant un ou plusieurs serveurs et sert d'intermédaires entre les clients et les serveurs).

Un fichier d'IngressRoute est par exemple:

``` yaml
apiVersion: traefik.io/v1alpha1
kind: IngressRoute
metadata:
  name: nom_de_ton_ingressRoute
spec:
  entryPoints:
    - websecure
  routes:
    - match: Host("example.n1a.viarezo.fr")
      kind: Rule
      services:
        - name: nom_de_ton_service
          port: 80
  tls:
    secretName: nom_de_ton_secret
```

Pour la partie `routes`, on indique le service vers lequel il faut rediriger pour le nom de domaine donné. Dans la partie `tls`, on indique que du HTTPS sera utilisé en précisant le certificat associé (en précisant le nom du secret).

Le certificat est généré avec un `Certificat`, dont un exemple est:

```yaml
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: nom_de_ton_certificat
spec:
  secretName: nom_de_ton_secret # Accorder avec le TLS de l'IngressRoute
  dnsNames:
  - example.n1a.viarezo.fr
  issuerRef:
    name: letsencrypt-prod
    kind: ClusterIssuer
    group: cert-manager.io
```

Cela utilise le plugin cert-manager et Let's Encrypt comme autorité de certification.

Dernière étape: ajouter le nom de domaine dans le DNS:

En l'état, si on rentre le nom de domaine dans le navigateur, rien ne va se passer. 

Traefik fonctionne comme un loadbalancer, c'est à dire que le service possède une IP extérieur pour faire le lien avec l'extérieur.

Pour résoudre le nom de domaine, il faut ainsi rediriger les requêtes vers le nom de domaine, vers le service traefik qui se chargera ensuite de rediriger à l'intérieur du cluster.

Si on a telle configuration:

```yaml
origin: n1a.viarezo.fr.
soa:
  ns: ns1.viarezo.fr.
  user: support.ml.viarezo.fr.
ttl: 1h
records:
  - host: "@"
    type: NS
    value: ns1.viarezo.fr.
  - host: "@"
    type: NS
    value: ns2.viarezo.fr.
  - host: traefik
    type: A
    value: 138.195.139.105
```

et que l'on souhaite rajouter une nouvelle entrée, on rajouter à la fin des records:

```
  - host: example
    type: CNAME
    value: traefik
```

Continous and automated deployment is also possible on Kubernetes using tools like ArgoCD.

The setup of automatic deployment of an app using ArgoCD can be found [[Deployment example with Argo CD|here]].

Kubernetes can be provided as well by cloud providers like AWS or Azure.

The core concepts of Azure Kubernetes Service can be found [[Core concepts AKS|here]].

For the core concepts of Amazon Elastic Kubernetes Service can be found [[Core concepts AKS|here]] and a deployment example over [[Deployement of an app in AWS EKS|here]].