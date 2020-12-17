# Deploy Keycloak to Heroku
Ce référentiel déploie une image docker de Keycloak (SSO - Single Sign-On) sur Heroku. Il est basé sur l'image de docker officielle de Keycloak avec quelques légères modifications pour utiliser les variables Heroku `PORT` et `DATABASE_URL` correctement.

Le déploiement se fera avec un dyno gratuit (type: free) et créera une base de données Postgres gratuite attachée. Il est possible de choisir le type de dyno. Dans ce cas, je vous conseille de lire https://devcenter.heroku.com/articles/dyno-types pour choisir celui que vous voulez. Il est possible également de changer la taille de la base de données. Renseignez vous sur les différents types de taille ici https://devcenter.heroku.com/articles/heroku-postgres-plans.

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.svg)](https://heroku.com/deploy)

# Procédure
Pour le déploiement de ce service, nous avons utilisé l'image docker de keycloack. 

1. Fork ce dépôt
2. Clonez le dépôt fork afin d'effectuer les prochaines manipulations sur votre machine local
3. [ opt ] Si vous souhaitez changer le type de dyno, dans ce cas il faut modifier dans le fichier `app.json`, la valeur de `size: free` par le type de dyno voulu
4. [ opt ] Si vous souhaitez changer le type de base de données, dans ce cas il faut modifier dans le fichier `app.json`, la valeur de `plan: heroku-postgresql:<PLAN_NAME>` & modifier de la même manière la valeur dans le fichier *heroku.yml*
5. Modifier les droits d'exécution dans le container docker pour le fichier docker-entrypoint.sh pour le Dockerfile
>git add --chmod=+x -- docker-entrypoint.sh
6. On met à jour le dépôt distant :
>git add .

>git commit -am "Personnalisation du déploiement Keycloak"

>git push -u origin master
7. Rendez-vous dans le dépôt distant, dans l'affichage du readme vous verrez apparaître un bouton violet `Deploy to Heroku`, cliquez dessus !
8. Renseignez les différents champs, puis déployer
9. Allez sur votre gestionnaire d'application Heroku puis accédez aux variables de l'application déployer : `votre-application-keycloak > settings > Reveal config vars`
10. Ajoutez une nouvelle variable `key= DATABASE_URL` et vous allez copier/coller la valeur de la variable existante `HEROKU_POSTGRESQL_URL` dans la valeur de `DATABASE_URL`
11. Relancez le dyno dans l'onglet `ressource`, onglet 'pen', décochez puis cochez et confirmez. Regarder les logs, vous verrez peut-être des erreurs de type R10 out of memory mais l'application devrais être déployer avec succès.


# Architecture du dépôt
*app.json*              : fichier de configuration de metadata pour le déploiment du server sur heroku à partir du bouton de déploiement.

*config.json*           : Contient les variables d'env pour *app.json*

*docker-entrypoint.sh*  : Script bash pour la configuration de keycloak.

*Dockerfile*            : Indique quelle image docker de Keycloak utilisé, quel fichier bash (*docker-entrypoint.sh*) le container va utiliser pour se construire et enfin quel commande le dynos utilisera pour démarrer le serveur jboss.

*heroku.yml*            : Script pour dire à heroku de build une image docker avec un add-on de base de données. *cf : https://devcenter.heroku.com/articles/build-docker-images-heroku-yml*