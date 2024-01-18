# Accéder au wordpress via un nom de domaine

> ⚠️ Vous serez en complète autonomie. Vous allez devoir comprendre et utiliser un outil permeetant d'accéder 
> à une app depuis une url en local. On appelle communément un [reverse proxy](https://fr.wikipedia.org/wiki/Proxy_inverse). 

Dans cette partie, nous allons reprendre l'exercice sur wordpress.
On souhaite accéder au site web via l'url http://wordpress.localhost 

## Créer le compose.yaml du reverse proxy

Dans cette section nous allons utiliser le reverse proxy [Traefik](https://doc.traefik.io/traefik/)

Créez un répertoire `traefik` et à l'intérieur de celui-ci créez le fichier `compose.yaml`:

Ce fichier défini 1 service nommé `reverse-proxy`. Chercher dans la doc comment créer un reverse proxy et chercher comment permettre la communication entre votre app et votre reverse-proxy.

> _Indice:_ Vous allez devoir comprendre comment fonctionne les [labels](https://docs.docker.com/config/labels-custom-metadata/) sur Docker.

## Exigences

- Vous avez un fichier `compose.yaml` pour l'app wordpress et un `compose.yaml` pour le reverse-proxy
  - **Il ne sera pas autorisé** de définir les services dans **un même fichier**
- Après avoir effectué les commandes `docker compose up -d` pour les deux fichiers, vous devriez être en mesure d'accéder au wordpress via l'url `http://wordpress.localhost`

> _Indice:_ pour ajouter un nom de domaine local il faut modifier le fichier `/etc/hosts` (`C:\windows\system32\drivers\etc\hosts` pour windows)
