# Projet de déploiement de WordPress avec Kubernetes

Ce projet vise à déployer WordPress en utilisant Kubernetes. Il utilise des déploiements, des services et des volumes pour créer une application WordPress fonctionnelle avec une base de données MySQL.

## Prérequis

Avant de commencer, assurez-vous d'avoir les éléments suivants :

- Un cluster Kubernetes ou Minikube fonctionnel
- L'outil `kubectl` configuré pour se connecter au cluster

## Instructions

Suivez les étapes ci-dessous pour déployer WordPress avec Kubernetes :

1. Créez un namespace :
   - Utilisez le fichier yml fourni (`namespace.yml`) pour créer un namespace spécifique pour ce projet en exécutant la commande suivante : `kubectl apply -f namespace.yml`

2. Déployez MySQL :
   - Utilisez le fichier yml fourni (`mysql-deployment.yml`) pour déployer MySQL en exécutant la commande suivante : `kubectl apply -f mysql-deployment.yml`

3. Créez un service pour MySQL :
   - Utilisez le fichier yml fourni (`service-clusterip-mysql.yml`) pour créer un service ClusterIP pour exposer les pods MySQL en exécutant la commande suivante : `kubectl apply -f service-clusterip-mysql.yml`

4. Déployez WordPress :
   - Utilisez le fichier yml fourni (`wordpress-deployment.yml`) pour déployer WordPress en exécutant la commande suivante : `kubectl apply -f wordpress-deployment.yml`

5. Créez un service pour WordPress :
   - Utilisez le fichier yml fourni (`service-nodeport-wordpress.yml`) pour créer un service NodePort pour exposer le frontend de WordPress en exécutant la commande suivante : `kubectl apply -f service-nodeport-wordpress.yml`

6. Accédez à WordPress :
   - Utilisez la commande `hostname -I` pour obtenir l'adresse IP de votre cluster.
   - Ouvrez un navigateur web et accédez à l'adresse IP du cluster avec le NodePort spécifié pour le service WordPress dans le fichier `service-nodeport-wordpress.yml`.
    <br>

   ![proof-wordpress](https://github.com/MozkaGit/kubernetes-wordpress/assets/43102748/023003ee-cc9a-45ed-9e3e-3623536a2f8a)


## Preuve du montage du volume

Le déploiement de WordPress utilise un volume monté pour stocker les données dans le répertoire `/data` du nœud. Vous pouvez vérifier que le volume a bien été monté en exécutant la commande suivante :

```shell
kubectl get pods -n wordpress
kubectl describe pod <nom_du_pod> -n wordpress
```

Dans les détails du pod, vous devriez voir une section indiquant le montage du volume:

```
Volumes:
  volume:
    Type:          HostPath (bare host directory volume)
    Path:          /data
    HostPathType:  DirectoryOrCreate
```

Il est également possible de vérifier la présence des fichiers essentiels à WordPress dans le répertoire `/data` du nœud.:

![volume-proof](https://github.com/MozkaGit/kubernetes-wordpress/assets/43102748/06de7691-23fc-425d-b6a2-d0b92344f290)


## Architecture du projet

- `namespace.yml` : Fichier yml pour la création d'un namespace spécifique pour le projet.
- `mysql-deployment.yml` : Fichier yml pour le déploiement de MySQL.
- `service-clusterip-mysql.yml` : Fichier yml pour la création d'un service ClusterIP pour MySQL.
- `wordpress-deployment.yml` : Fichier yml pour le déploiement de WordPress.
- `service-nodeport-wordpress.yml` : Fichier yml pour la création d'un service NodePort pour WordPress.

## Remarques

- Assurez-vous que votre cluster Kubernetes ou Minikube est configuré correctement avant de commencer le déploiement.
- Vous pouvez personnaliser les valeurs, les variables d'environnement et les options de volumes selon vos besoins dans les fichiers de déploiement yml.
- Veillez à sécuriser les informations sensibles telles que les mots de passe en utilisant des secrets Kubernetes.
