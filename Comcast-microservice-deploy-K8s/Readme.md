This repo contains a simple micro service in Python that returns the weather for a given city and deployed it to Kubernetes cluster (EKS Cluster)

Step:1 Use OpenWeatherMap API 

How to get API key (APPID): To get access to weather API we need an API key, for that need  to signup OpenWeatherMap to get unique API key from account page.

API call:
	http://api.openweathermap.org/data/2.5/forecast?id=524901&APPID={APIKEY}

Parameters:
	APPID {APIKEY} is our unique API key 

Example of API call:
	api.openweathermap.org/data/2.5/forecast?id=524901&APPID=1111111111 


Step:2 Python Script (comcast-weather.py) to return weather for given city.
	
Created Simple Python script for dealing with Weather API that returns weather Information for the CITY Passed as an input.


Step:3 Create K8S cluster (eksctl - a CLI for Amazon EKS)
eksctl is a simple CLI tool for creating clusters on EKS - Amazon's new managed Kubernetes service for EC2. It is written in Go, and based on Amazon's official CloudFormation templates.

Usage:

To download the latest release, run:
curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp sudo mv /tmp/eksctl /usr/local/bin


To create a basic cluster, run:

eksctl create cluster
A cluster will be created with default parameters

 2x m5.large nodes 
 default EKS AMI
 us-west-2 region
 dedicated VPC

Once you have created a cluster, you will find kubeconfig in your current working directory. 
If you have kubectl v1.10.x as well as heptio-authenticator-aws commands in your PATH, you should be able to use kubectl. 

To list the details about a cluster or all of the clusters, use:
eksctl get cluster [--name <name>] [--region <region>]

To create the same kind of basic cluster, but with a different name, run:
eksctl create cluster --name cluster-1 --nodes 4

To write cluster credentials to a file other than default, run:
eksctl create cluster --name cluster-2 --nodes 4 --kubeconfig ./kubeconfig.cluster-2.yaml

To prevent storing cluster credentials locally, run:
eksctl create cluster --name cluster-3 --nodes 4 --write-kubeconfig=false

To let eksctl manage cluster credentials under ~/.kube/eksctl/clusters directory, run:
eksctl create cluster --name cluster-3 --nodes 4 --auto-kubeconfig

To obtain cluster credentials at any point in time, run:
eksctl utils write-kubeconfig --name <name> [--kubeconfig <path>]

To use a 3-5 node Auto Scaling Group, run:
eksctl create cluster --name cluster-5 --nodes-min 3 --nodes-max 5

To use 30 c4.xlarge nodes, run:
eksctl create cluster --name cluster-6 --nodes 30 --node-type c4.xlarge

To delete a cluster, run:
eksctl delete cluster --name <name> [--region <region>]



