# This project is for the Devops bootcamp exercise for "Kubernetes on AWS - EKS"

Right after you setup the cluster on LKE or Minikube and deployed your application inside, your manager comes to you to say, that the company wants to run Kubernetes also on AWS, again less overhead when managing just one platform. So they ask you to reconfigure your cluster on AWS and deploy your application there instead.



## EXERCISE 1: Create EKS cluster

You decide to create an EKS cluster - the managed Kubernetes Service of AWS. To simplify the whole creation and configurations, you use eksctl.

* With eksctl you create an EKS cluster with 3 Nodes and 1 Fargate profile

**Solution:**

The command line tool can be downloaded [here](https://eksctl.io/).

    # First we create the cluster using the following command.
    # The access configurations are stored in the yaml file.
    eksctl create cluster \
    --name demo-cluster \
    --version 1.25 \
    --region eu-central-1 \
    --nodegroup-name demo-nodes \
    --node-type t2.micro \
    --nodes 3 \
    --kubeconfig=./kubeconfig.demo-cluster.yaml \
    --nodes-min 1 \
    --nodes-max 3

    # In order to point the Kubectl to the cluster:
    export KUBECONFIG={absolute-path}/kubeconfig.demo-cluster.yaml

    kubectl get node
     # NAME                                              STATUS   ROLES    AGE     VERSION
     # ip-192-168-16-181.eu-central-1.compute.internal   Ready    <none>   5m52s   v1.25.7-eks-a59e1f0
     # ip-192-168-60-5.eu-central-1.compute.internal     Ready    <none>   5m55s   v1.25.7-eks-a59e1f0
     # ip-192-168-75-235.eu-central-1.compute.internal   Ready    <none>   5m48s   v1.25.7-eks-a59e1f0