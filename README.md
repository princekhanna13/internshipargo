# internshipargo

## How to run

### run the following commands :

- ``kubectl create namespace argocd``   ->  create the namespace for argocd
- ``kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml``     ->  install argocd from the repository on argocd ns


- ``kubectl create -f argo.yaml``   ->    create the main application

- ``kubectl apply -f argocd-cm.yaml``    ->    change the configmap for argocd ( in this case to lower the auto-sync time )

- ``kubectl -n argocd rollout restart deploy argocd-repo-server``     ->    the server needs to be restarded after the changes are made


### to test the auto-sync time use :

- ``kubectl -n argocd get configmap argocd-cm -o jsonpath='{.data.timeout\.reconciliation}'``

#### or to get all the configuration setting :

- ``kubectl describe configmaps argocd-cm -n argocd``


## log in 

### get the password using the following command and the username will be ```admin```

- ``kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo``
