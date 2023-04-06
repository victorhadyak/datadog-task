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

# Step 3: Deploy the Datadog Agent:
`helm install RELEASE_NAME -f datadog-values.yaml --set datadog.site='us5.datadoghq.com' --set datadog.apiKey=<DATADOG_API_KEY> datadog/datadog`

# Step 4: Customize Datadog dashboards.



# Conclusion
In this project, I created a Datadog Monitoring for Nginx Kubernetes
