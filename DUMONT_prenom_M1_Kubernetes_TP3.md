Rendu TP2

Thomas Dumont

M1
___

1. Un pod MariaDB

Pour le fichier de conf, voir "conf_mariadb.yaml".

Afin de lancer le pod :

`kubectl create -f conf_mariadb.yaml`

2. Un PVC pour stocker les données MariaDB

Partie du fichier de conf permettant de configurer un PVC :
```
volumes:
  - name: maria-db
    persistentVolumeClaim:
      claimName: maria-db
```

```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: maria-db
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

3. Mettre à jour le pod du serveur pour se connecter au service MariaDB :

If faut ajouter `namespace: default` dans la partie `metadata:` du fichier de configuration du pod deno-webserver.

Afin d'accéder au fichier de configuration du pod deno-webserver il faut faire la commande suivante :

`kubectl edit deployment deno-webserver`