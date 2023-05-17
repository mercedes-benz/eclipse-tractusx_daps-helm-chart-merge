## Installation Steps

Helm charts are provided inside https://github.com/eclipse-tractusx/daps-helm-chart

1.) Using helm commands: <br />

How to install application using helm: <br />
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
    c.) Add the image.repository in the values file (You can use your own images, one available image is available at the frauenhofer      ghcr.io/fraunhofer-aisec/omejdn-server:1.7.1")

    d.) Deploy in a kubernetes cluster
        helm install daps-server charts/daps-server/ -n NameSpace
