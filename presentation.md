---

class: center, middle
type: slide
slideOptions:
    transition: 'concave'
    backgroundTransition: 'convex'
    transitionSpeed: 'default'
    parralaxBackgroundImage:
    parralaxBackgroundHorizontal: 400
    parralaxBackgroundVertical: 400
    
---

  <!-- .slide: data-background="https://hackmd.io/_uploads/ryG_0BVFh.png" data-background-size="500px" -->
 <span style="color:yellow">  Mise en œuvre de Traefik Proxy pour sécuriser l'accès à l'application Voting App
</span>

---

# Installation de Traefik

- Ajouter le Chart Helm de Traefik dans le repository:
    - helm repo add traefik https://helm.traefik.io/traefik
    - helm repo update

- Déploiment de Traefik avec Helm dans le cluster AKS:
    - helm install traefik traefik/traefik

---

  <!-- .slide: data-background="https://hackmd.io/_uploads/ryG_0BVFh.png" data-background-size="500px" -->

# BasicAuth HTTP

----

## Objectif
- Pour sécuriser l'application ajout d'une basic auth user:password

----

## Process
- Création d'un secret avec combinaison user:password
- Ajout d'un middleware de basic auth pour Traefik
- Ajout dans l'ingress d'une annotation pour activer la basic auth

----

## Difficultés 
- Gestion du navigateur pour test (global)
- Mise en place de Traefik
- Syntaxe des annotations middleware pour l'ingress

---

  <!-- .slide: data-background="https://hackmd.io/_uploads/ryG_0BVFh.png" data-background-size="500px" -->

# Filtrage d'IP

----

## Objectif
- Pour autoriser certaines IP mise en place d'une whitelist

----

## Process
- Ajout d'un middleware de type ipwhitelist
- Ajout d'une annotation dans l'ingress

---

  <!-- .slide: data-background="https://hackmd.io/_uploads/ryG_0BVFh.png" data-background-size="500px" -->

# Certificats TLS

----

## Objectif
- Mise en place d'un certificat TLS pour renforcer la sécurité de l'appli

----

## Process
- Installation de cert manager sur le cluster
- Création d'un Issuer pour générer le certificat signé par let's encrypt
- Ajout dans l'ingress d'annotation de cert-manager

----

## Difficultés
- Comprendre le mécanisme derrière le issuer de cert-manager

---

  <!-- .slide: data-background="https://hackmd.io/_uploads/ryG_0BVFh.png" data-background-size="500px" -->

# Authentification avec Certificat TLS Client

----

## Objectif

- Authentification avec un certificat client unique validé par une autorité de certification.

----

## Process
- Création d'une clé et un certificat d'autorité déposé grace à un secret sur le cluster
- Création d'un TLSOption qui utilise le secret et configure traefik pour vérifier le certificat client
- Création d'une nouvelle clé, une nouvelle requête de certificat et d'un nouveau certificat
- Validation par l'autorité de certification du certificat généré en pfx
- Création d'un pfx à partir du certificat afin de l'importer sur firefox
- Le pfx est ensuite importé dans firefox pour donner l'accés

----

## Difficultés

- Compréhension de la gestion des certificats
- Gestion des secrets

---

# Authentification par SSO Google

----

## Objectifs
- Ne plus passer par l'authenfication basique mais par l'authentification Google

----

## Process
- Création d'un project google cloud
    - Création page de consentement
    - Création Oauth id
- Création d'un secret avec les élements de connexion google
- Déploiement d'un pod pour gérer le sso avec Traefik
- Mise en place d'un middleware sso
- Ajout d'annotations dans l'ingress

----

## Difficultés
- Gestion de la mise en forme du secret