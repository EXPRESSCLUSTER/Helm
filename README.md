# Helm

## Setup Helm
1. If your computers behind proxy server, you need to run the following command in advance.
   ```sh
   $ export http_proxy=<proxy server>:<port number>
   $ export https_proxy=$http_proxy
   ```
1. Download Helm package from https://github.com/helm/helm/releases/.
   ```sh
   $ curl -O https://get.helm.sh/helm-v2.16.5-linux-amd64.tar.gz
   ```
1. Expand archive and copy a binary file.
   ```sh
   $ tar xzvf helm-v2.16.5-linux-amd64.tar.gz
   $ cp linux-amd64/helm /usr/local/bin
   ```
1. Create yaml file (e.g. tiller-rbac.yaml) as below.
   ```yaml
   apiVersion: v1
   kind: ServiceAccount
   metadata:
     name: tiller
     namespace: kube-system
   ---
   apiVersion: rbac.authorization.k8s.io/v1
   kind: ClusterRoleBinding
   metadata:
     name: tiller
   roleRef:
     apiGroup: rbac.authorization.k8s.io
     kind: ClusterRole
     name: cluster-admin
   subjects:
     - kind: ServiceAccount
       name: tiller
       namespace: kube-system   
   ```
1. Create RBAC.
   ```sh
   $ kubectl create -f tiller-rbac.yaml
   ```
1. Init Helm.
   ```sh
   $ helm init --service-account tiller
   ```
1. Check Helm version.
   ```sh
   $ helm version
   Client: &version.Version{SemVer:"v2.16.5", GitCommit:"89bd14c1541fa93a09492010030fd3699ca65a97", GitTreeState:"clean"}
   Server: &version.Version{SemVer:"v2.16.5", GitCommit:"89bd14c1541fa93a09492010030fd3699ca65a97", GitTreeState:"clean"}   
   ```