# This project is for the Devops bootcamp exercise for "Kubernetes on AWS - EKS"

Right after you setup the cluster on LKE or Minikube and deployed your application inside, your manager comes to you to say, that the company wants to run Kubernetes also on AWS, again less overhead when managing just one platform. So they ask you to reconfigure your cluster on AWS and deploy your application there instead.



## EXERCISE 1: Create EKS cluster

You decide to create an EKS cluster - the managed Kubernetes Service of AWS. To simplify the whole creation and configurations, you use eksctl.

* With eksctl you create an EKS cluster with 3 Nodes and 1 Fargate profile