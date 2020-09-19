Kubernetes as known is the open source container orchestration tool that is widely used in all over the world. Ant Media Server even supports running in Docker container, it has some kind of incompatibility issues when it's running in Kubernetes. With the version 2.2.0+, Ant Media Server is fully compatible with Kubernetes. Let us share how to use Ant Media Server with Kubernetes.

Running Ant Media Server is fully about clustering. If you are not familiar with Ant Media Server Clustering & Scaling, please read the [Cluster & Scaling documentation](Clustering-&-Scaling)

Btw, the scope of this document is giving you the basics about how to run Ant Media Server Kubernetes Cluster. If you're not familiar with Kubernetes then you can get started with [Kubernetes](https://kubernetes.io/docs/home/) and follow [interactive tutorials](https://kubernetes.io/docs/tutorials/kubernetes-basics/create-cluster/cluster-intro/).

After these notes, just before proceeding we assume that you've installed kubernetes in your test environment or in a cloud service like AWS. The scenario below is generally running the Kubernetes with Minikube. It's also tested in AWS EKS as well. If you use minikube, we start the it with `sudo minikube start --driver=none` in VPS and need to use `sudo` command in `docker` and `kubectl`. On the other hand, you don't need to use `sudo` in `kubectl` commands if you're running in AWS. 

## 1. Create Image for Container
First things first. We need to create a docker image to run our pods in kubernetes.

* Get the Dockerfile. Dockerfile is available in Ant Media Server's Scripts repository that is actively used in CI pipeline. Anyway, you can get it with the below command. 
  ```
  wget https://raw.githubusercontent.com/ant-media/Scripts/master/docker/Dockerfile_Process \
  -O Dockerfile_Process
  ``` 
* Download or Copy Ant Media Server Enterprise Edition ZIP file into the same directory that you download Dockerfile above. 

* Create the docker image. Before running the command below, please pay attention that you should replace `{CHANGE_YOUR_ANT_MEDIA_SERVER_ZIP_FILE}` in the command below with your exact Ant Media Server ZIP file name.

  ```
  sudo docker build --network=host --file=Dockerfile_Process -t ant-media-server-enterprise-k8s:test \
  --build-arg AntMediaServer={CHANGE_YOUR_ANT_MEDIA_SERVER_ZIP_FILE} .
  ```

  The second thing we should point out is the image name and tag. The command above use the `ant-media-server-enterprise-k8s:test` as image name and tag. The image name is compatible with deployment file. I mean you can absolutely change the image name and tag, just make it compatible with deployment file we'll mention soon. 

If everything is OK, your image is available in your environment. If you're going to use this image in AWS EKS or similar service, you need to upload the image to repository such as [AWS ECR](https://aws.amazon.com/ecr/) or you can run a [local registry](https://docs.docker.com/registry/deploying/#run-a-local-registry).   

## 2. Run Ant Media Server K8s Deployment

* Download the `ams-k8s-deployment.yaml` file with the following command. 
  ```
  wget https://raw.githubusercontent.com/ant-media/Scripts/master/kubernetes/ams-k8s-deployment.yaml -O ams-k8s-deployment.yaml
  ```
* Change the deployment file according to your needs. In order to do that, let us give some more information about the `ams-k8s-deployment.yaml`  The content of the file is like this
  ```yml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: ant-media-server
  spec:
    selector:
      matchLabels:
        run: ant-media-server
    replicas: 1
    template:
      metadata:
        labels:
          run: ant-media-server
      spec:
        hostNetwork: true
        containers:
        - name: ant-media-server
          imagePullPolicy: IfNotPresent  # change this value accordingly. It can be Never, Always or IfNotPresent
          image: ant-media-server-enterprise-k8s:test  #change this value according to your image.
  # You basically need to update the mongodb server url(-h) below. You may also need to add -u and -p parameters for
  # specifying mongodb username and passwords respectively 
          args: ["-g", "true", "-s", "true", "-r", "true", "-m", "cluster", "-h", "127.0.0.1"]  
  ```

  We should point out the somethings **important** in the file above.
  * `hostNetwork: true` line above means that Ant Media Server uses the host network. It is required because there is a wide range of UDP and TCP ports are being used for WebRTC streaming. This also means that you can only use one pod of Ant Media Server in a host instance. You don't worry about the where and how to deploy, K8s handles that. We're just letting you know this information to determine total number of nodes in your cluster.   
  * `imagePullPolicy: IfNotPresent` means that if the image is available in local environment, it'll not pulled from the private or public registry. 
  * `image: ant-media-server-enterprise-k8s:test` specifies the name of the image. You should pay attention here it should be the same name with the image you build in previous step.
  * `args: ["-g", "true", "-s", "true", "-r", "true", "-m", "cluster", "-h", "127.0.0.1"]` specifies the parameters for running the Ant Media Server pods. Let us tell their meanings and why we need them.
    * `"-g", "true"`: It means that Ant Media Server uses the public IP address of the host for internal cluster communication. Default value is false. 
    * `"-s", "true"`: It makes Ant Media Server uses its public IP address as server name.
    * `"-r", "true"`: It makes Ant Media Server replaces the local IP address in the ICE candidates with the server name. It's false by default.
    * `"-m", "cluster"`: It specifies the server mode. It can be `cluster` or `standalone`. Its default value is `standalone`. If you're running Ant Media Server in kubernetes, it's most likely you're running the Ant Media Server in cluster mode. Which means that you need to specify the your MongoDB host, username and password as parameter. 
    * `"-h", "127.0.0.1"`: It specifies the MongoDB host address. It's necessary to use if you're running in cluster mode. In this example, it's `127.0.0.1` because in the CI pipeline, local MongoDB is installed. You should change it with your own MongoDB address or replica set. 
    * `"-u", "username"`: It specifies the username to connect to the MongoDB. If you don't have credentials, you don't need to specify. 
    * `"-p", "password"`: It specifies the password to connect to the MongoDB. If you don't have credentials, you don't need to specify.

  So you can now change the `ams-k8s-deployment.yaml` file according to your environment. 

* Run the `ams-k8s-deployment.yaml` with following command. 
  ```
  sudo kubectl create -f ams-k8s-deployment.yaml
  ```
  If everything is OK, you'll have a running deployment. You can query your deployment with the following command. 
  ```
  sudo kubectl get deployments
  ```
  Then you should see something like below.
  ```
  NAME               READY   UP-TO-DATE   AVAILABLE   AGE
  ant-media-server   1/1     1            1           12s
  ```

## 3. Run Ant Media Server K8s Service
Running Ant Media Server K8s Service is the easiest part in pipeline.
* Download the K8s Service file
  ```
  wget https://raw.githubusercontent.com/ant-media/Scripts/master/kubernetes/ams-k8s-service.yaml -O ams-k8s-service.yaml
  ```
* Create the Service
  ```
  sudo kubectl create -f ams-k8s-service.yaml
  ```
  Check the service if it's working.
  ```
  sudo kubectl get services
  ```
  If it's running, you should see something similar like this.
  ```
  NAME               TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
  ant-media-server   LoadBalancer   10.105.169.89   <pending>     5080:30240/TCP   6s
  kubernetes         ClusterIP      10.96.0.1       <none>        443/TCP          6m50s
  ```

  If you're running with minikube, you can directly connect to your platform with your host ip address in the browser and the port name above. Let's assume your host IP address is `xxx.yy.yy.zz`, then you can connect to Ant Media Server web panel via `http://xxx.yy.yy.zz:30240`. If you're running this service in AWS or any other place, please read their documentation about Load Balancer in Kubernetes. 

 