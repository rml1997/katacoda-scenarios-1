# Activities that we are going to perform

We have our cluster up and running, now we are going to follow the next steps to deploy our application on top of it:

1. Download the code
2. We are going to create a namespace where we will deploy our application. We will learn more about namespaces later on.
3. Our first deployment will be the Redis cluster, using a StatefulSet (YML)
4. On top of the Redis Cluster we will deploy a service (YML)
5. We are going to use a Docker file that will contain our main.go application
6. With the Docker image created, we will register it on Oracle Container Registry (OCIR)
7. Once register, we are going to deploy our Go application (API) from a yml file
8. We will expose the service

## Download the code

`git clone https://github.com/manraog/redis-session-api.git`{{execute}}

All credits on this code to Ricardo Ortega (a.k.a "el muchacho").

## Namespace creation

Namespaces are created to organize what you deploy on top of your cluster. In the Kubernets documentation you will read that namespaces are like virtual clusters
deployed on the same physical cluster.
The intention to create namespaces is to be used in environments where you have differente users working in different teams and projects.

After you created your cluster, you will have a set of default namespaces. Exeute this to get the list:

`kubectl get namespaces --insecure-skip-tls-verify`{{execute}}

~~~~
NAME              STATUS   AGE
default           Active   17d
kube-node-lease   Active   17d
kube-public       Active   17d
kube-system       Active   17d
~~~~

Those are the default namespaces. 

For our scenario we will create the namespace: ociLab + $LAB_ID

`export NAMESPACE=ocilab$LAB_ID`{{execute}}

Now let's create the namespace:

`kubectl create namespace $NAMESPACE --insecure-skip-tls-verify`{{execute}}

If you re-issue the `kubectl get namespaces --insecure-skip-tls-verify`{{execute}}  you will see your newly created namespace listed:

~~~~
NAME              STATUS   AGE
default           Active   17d
kube-node-lease   Active   17d
kube-public       Active   17d
kube-system       Active   17d
ocilab$LAB_ID     Active   11s    <---
~~~~

In order to deploy your artifacts to the ocilab$LAB_ID namespace, you just need to include the option -n $NAMESPACE within the kubectl commands. 

For example, to get the list of pods in the $NAMESPACE, you will do:

`kubectl get pods -n $NAMESPACE --insecure-skip-tls-verify`{{execute}}

![OKE Create Cluster](/RedExpertAlliance/courses/oci-course/oke-redis-cache-and-functions-oci/assets/11.jpg)