## Installation Steps

Helm charts are provided inside https://github.com/eclipse-tractusx/daps-helm-chart

1.) Using helm commands:- <br />

How to install application using helm:-
    helm install ReleaseName ChartName
    
    a.) Add helm repository in tractusx:-
           helm repo add daps-server https://eclipse-tractusx.github.io/charts/dev
    b.) To search the specific repo in helm repositories 
           helm search repo tractusx-dev
    c.) To install using helm command:-   
           helm install daps-server tractusx-dev/daps-server


2.) Local installation:

    a.) git clone https://github.com/eclipse-tractusx/daps-helm-chart.git <br />
    b.) Modify values file according to your requirement.  <br />
    c.) Add the image.repository in the values file
    c.) You need to define the secrets as well in values.yaml  <br />
        secret:  <br />
          clientId:  -> Client id for DAPS.   
          clientSecret:   -> Client Secret for DAPS  <br />

    d.) These secrets should be defined in Hashicorp vault. <br />
    e.) Deploy in a kubernetes cluster  <br />
        helm install daps-server charts/daps-server/ -n NameSpace  <br />