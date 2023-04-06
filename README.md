# Datadog Monitoring for Nginx Kubernetes
This project provides step-by-step instructions for deploying an Datadog monitoring in a Kubernetes cluster.

# Introduction
Example of running the Datadog Agent in a Kubernetes cluster directly in order to start collecting your cluster and applications metrics, traces, and logs. You can deploy it with a Helm chart (used in this example), the Datadog Operator or directly with a DaemonSet.

# Prerequisites
Datadog account, Helm

# Step 1: Install Helm.
- `curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm`
- Add the Datadog Helm repository: helm repo add datadog `https://helm.datadoghq.com`
- Fetch the latest version of newly added charts: `helm repo update`

# Step 2: Create a [YAML](./values.yaml) file.
Create a yaml file, and override any of the  [default](./default.yaml) values if desired.

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
