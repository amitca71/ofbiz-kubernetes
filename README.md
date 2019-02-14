# ofbiz-kubernetes
deploy ofbiz + nginx on GKE
The following describes creating ofbiz on kubernetes, using derby database and ofbiz demo data
Each and every Deployment (pod), contains nginx and ofbiz containers, that are exposed as a service, via ingress load balancer

1) create google cloud project (https://cloud.google.com/resource-manager/docs/creating-managing-projects)
1) login to google cloud:
gcloud auth login
follow the instructions (login to browser, get the verification code...)

2) install kubectl (e.g for ubuntu 18.04: https://linuxconfig.org/how-to-install-kubernetes-on-ubuntu-18-04-bionic-beaver-linux)

3) create kubernetes cluster (see parameters on https://cloud.google.com/sdk/gcloud/reference/container/clusters/create. eg:
sudo gcloud container clusters create ofbiz --num-nodes 1 --machine-type  n1-standard-2 --zone=europe-west1-b --network=default 

4) get credentials:
sudo gcloud container clusters get-credentials ofbiz --zone europe-west1-b --project [GOOGLE-PROJECT-NAME]

5) create ingress tls secrets:
mkdir $HOME/secrets;
openssl genrsa -out $HOME/secrets/tls.key 2048
openssl req -new -x509 -key $HOME/secrets/tls.key -out $HOME/secrets/tls.cert -days 360 -subj /CN=mydomain.info
cd $HOME/secrets; 
sudo kubectl create secret tls tls-secret --cert=tls.cert --key=tls.key

6)create Deployment (pod):
sudo kubectl create -f nginx-ofbiz-deployment.yaml

7) create session affinity for the service (at time of writing, there is an open defect for google, that is under process.
see https://issuetracker.google.com/issues/124064870 for details)  
sudo kubectl create -f  backendconfig.yaml 

8) create service :
sudo kubectl create -f  nginx-ofbiz-service.yaml

9) create Ingress:
sudo kubectl create -f ingress-tls.yaml 

10) wait for the ingress to be ready (it might take few minutes), and check the IP (sudo kubectl get ingress)
for example: 
NAME    HOSTS   ADDRESS         PORTS     AGE
ofbiz   *       35.190.28.220   80, 443   16m

11) thats it, all set, you can now browse to https://[your-IP]/partymgr/control/main 

What is missing?
This is for demonstration only, and uses the ofbiz internal Derbi DB. for production you will use another one..(such postgres/mysql)
this will require adding additional container to the pod, as described on https://cloud.google.com/sql/docs/mysql/connect-kubernetes-engine
you are welcome for any questions: amitca71@gmail.com




