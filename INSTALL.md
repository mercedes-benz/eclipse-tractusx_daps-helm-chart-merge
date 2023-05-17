## Installation Steps

Helm charts are provided inside https://github.com/eclipse-tractusx/daps-helm-chart

1.) Using helm commands: <br />

How to install application using helm:
    helm install ReleaseName ChartName
    
    a.) Add helm repository in tractusx:
           helm repo add daps https://eclipse-tractusx.github.io/charts/dev
    b.) To search the specific repo in helm repositories 
           helm search repo daps/daps-server
    c.) To install using helm command:  
           helm install daps daps/daps-server


2.) Local installation:

    a.) git clone https://github.com/eclipse-tractusx/daps-helm-chart.git
    b.) Modify values file according to your requirement
    c.) Add the image.repository in the values file
    c.) You need to define the secrets as well in values.yaml
        secret:
          clientId:  -> Client id for DAPS   
          clientSecret:   -> Client Secret for DAPS

    d.) These secrets should be defined in Hashicorp vault
    e.) Deploy in a kubernetes cluster
        helm install daps-server charts/daps-server/ -n NameSpace
