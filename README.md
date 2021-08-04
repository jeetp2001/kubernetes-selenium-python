Steps for setting up docker and Kubernetes on Ubuntu:

-->sudo apt-get update
-->sudo apt-get install docker.io
-->sudo systemctl enable docker
-->sudo systemctl status docker
-->sudo apt-get install curl
-->sudo apt-get install apt-transport-https
-->sudo apt install virtualbox virtualbox-ext-pack (#install virtualbox for creating a virtual machine which is required to setup kubernetes cluster)
-->wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
-->sudo cp minikube-linux-amd64 /usr/local/bin/minikube
-->sudo chmod 755 /usr/local/bin/minikube
-->curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
-->sudo chmod +x ./kubectl
-->sudo mv ./kubectl /usr/local/bin/kubectl


Now, Minikube, kubectl and docker are installed on ubuntu. To start minikube cluster:

-->minikube start

Next create docker images using Dockerfile present in thread1,thread2,thread3 folders.

-->docker build -t image_name_1 /path/to/dockerfile/of/thread1/folder
-->docker build -t image_name_2 /path/to/dockerfile/of/thread2/folder
-->docker build -t image_name_3 /path/to/dockerfile/of/thread3/folder


Next push all these images to your dockerhub account.

For loging into your dockerhub account type foll. command:
-->docker login 
provide username and password

after running this command, run following commands to push docker images.
-->docker tag image_name_1 dockerhub_username/image_name_1
-->docker tag image_name_2 dockerhub_username/image_name_2
-->docker tag image_name_3 dockerhub_username/image_name_3
-->docker push dockerhub_username/image_name_1
-->docker push dockerhub_username/image_name_2
-->docker push dockerhub_username/image_name_3



Using these images create a yaml configuration files for creating deployments in kubernetes.
In my case, I have created 3 yaml configuration files named th1.yaml, th2.yaml, th3.yaml.

Create another configuration file for create deployment for postges container for storing fetched data.

After creating these configuration files, create deployment in kubernetes by the following commands:
-->kubectl create -f /location/of/yaml/configuration/file/of/postgres
-->kubectl create -f /location/of/yaml/configuration/file/of/image1
-->kubectl create -f /location/of/yaml/configuration/file/of/image2
-->kubectl create -f /location/of/yaml/configuration/file/of/image3

You can check your deployments using:
-->kubectl get deployments

For scaling the deployments:
-->kubectl scale deployment deployment_name --replicas=1 or any number

For stopping the deployments:
-->kubectl scale deployment deployment_name --replicas=0

For checking the values stored in postgres database, exec into the kubernetes postgres deployment/pod by using following command:
-->kubectl exec --stdin --tty pod_name -- /bin/bash

