# Datadog Monitoring for Nginx Kubernetes
This project provides step-by-step instructions for deploying an Datadog monitoring in a Kubernetes cluster.

# Introduction
Example of running the Datadog Agent in a Kubernetes cluster directly in order to start collecting your cluster and applications metrics, traces, and logs. You can deploy it with a Helm chart (used in this example), the Datadog Operator or directly with a DaemonSet.

# Prerequisites
Datadog account, Helm

Install Helm.
Add the Datadog Helm repository: helm repo add datadog https://helm.datadoghq.com
Fetch the latest version of newly added charts: helm repo update.
Create an empty values.yaml file, and override any of the default values if desired. Some examples can be found here.

# Step 1: Create a Dockerfile
`curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm`

# Step 2: Create a Kubernetes Deployment and Service YAML file
This [YAML](./Deployment.yaml) file creates a Deployment with two replicas of the Nginx forward proxy container. It also creates Readiness and liveness probes which are used by Kubernetes to check the health and availability of your application's containers. These probes help Kubernetes determine whether a container is ready to receive traffic (readiness) or if it's still alive and running correctly (liveness) and the strategy for deployment - there are two main strategies available in Kubernetes: RollingUpdate and Recreate. Kubernetes will use the RollingUpdate strategy by default for each deployment. In this example, I've added the strategy field under spec. The type is set to RollingUpdate. The rollingUpdate field allows you to configure maxSurge and maxUnavailable settings:
- maxSurge is the number of additional replicas that can be created during an update. This setting helps maintain the desired number of replicas during an update.
- maxUnavailable is the number of replicas that can be unavailable during an update. This setting helps control the rate of deployment.

With Recreate strategy, Kubernetes terminates all the existing Pods before creating new ones with the updated version. This approach causes downtime during the update process, as there will be a period when no replicas are available to serve request

# Step 3: Creating a Kubernetes Cluster using kops
This [Script](./kops.sh) install kops, kubectl, creates an S3 bucket to store the kops state and a Kubernetes cluster.
Kops is a tool for automating the deployment and management of Kubernetes clusters on AWS. It simplifies the process of creating, upgrading, and scaling Kubernetes clusters by providing a declarative API and command-line interface that abstracts away the complexity of setting up and configuring the underlying infrastructure.

# Step 4: Deploying and Testing the Nginx Forward Proxy
- `kubectl apply -f Deployment.yaml` - After validating the cluster apply the Kubernetes Deployment YAML file.
- `kubectl get pods` Check the status of the Nginx forward proxy pods using the following command.
- `kubectl get services` Obtain the IP address for the Nginx forward proxy service. This command displays a list of all the services running in your Kubernetes cluster, including the service for the Nginx forward proxy. Look for the EXTERNAL-IP field to obtain the IP address that clients can use to access the proxy.
- Test the Nginx forward proxy by entering the IP address in a web browser's proxy settings and visiting a website. 
- `curl --proxy http://<proxy-ip>:80 http://www.example.com` - Alternatively, test the Nginx forward proxy using the curl command.

# Conclusion
In this project, I created a Kubernetes deployment for an Nginx forward proxy, including services, and deployment strategies.
