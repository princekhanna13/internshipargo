# internshipargo

## How to run

### run the following commands :

- ``kubectl create namespace argocd``   ->  create the namespace for argocd
- ``kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml``     ->  install argocd from the repository on argocd ns


- ``kubectl apply -f argocd-cm.yaml``    ->    change the configmap for argocd ( in this case to lower the auto-sync time )

- ``kubectl create -f argo.yaml``   ->    create the main application


### to test the auto-sync time use :

- ``kubectl -n argocd get configmap argocd-cm -o jsonpath='{.data.timeout\.reconciliation}'``

#### or to get all the configuration setting :

- ``kubectl describe configmaps argocd-cm -n argocd``

##  before log in

### a port forward is needed

- ``kubectl port-forward svc/argocd-server -n argocd **YOUR_DESIRED_PORT**:443``


## log in 

### get the password using the following command and the username will be ```admin```

- ``kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo``   -> for unix systems(I think)
- ``kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}"``   ->  for windows
  ### then the password needs to be decoded from base64 fromat
  
## after log in

### you need to port-forward the vue, keycloak and node app

- ``kubectl port-forward svc/keycloak-service -n keycloak 8082:8082``
- ``kubectl port-forward svc/node-service -n node 8000:8000``
- ``kubectl port-forward svc/vue-service -n vue 3000:3000``


##  for the app to work completly there are a few settings that need to be done in keycloak

- go to ``localhost:8082`` and log in with user:``admin2`` and pass:``admin`` 
- add each real from ``./internshipargo/keycloak_realms/`` (one by one)
- then add a user in one of the realms with atributes: ``number`` and role mapinngs:```**company_ID**``` -> ``admin`` (**company_ID** will be replaced with the company name coresponding to the realm <<for example: lufthansaID>>)


## the app starts on ``localhost:3000``
