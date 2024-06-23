# eks-terraform

Note: I've used eks here, written in terraform to set-up the eks on aws.

Step-by-Step Documentation for Kubernetes Deployment and Scaling Project

1. **Setup Terraform Providers and Modules**

*Files:
- `providers.tf`
- `locals.tf`

Steps:
- Define the AWS provider: This sets up the connection to AWS.

- Set up local variables: Manage environment-specific configurations.

 2. **VPC and Subnets Configuration**

Files:
- vpc.tf
- subnets.tf

Steps:
- Create a VPC: Provides an isolated network for your infrastructure.
- Create public and private subnets: Divides your VPC into smaller, manageable sections.

Explanation: Creates the VPC and subnets as defined in the Terraform files.

 3. **EKS Cluster and Node Groups**

Files:
- eks.tf
- eks_nodes.tf

Steps:
- Create an EKS cluster: Manages Kubernetes control plane.
- Create node groups: Adds worker nodes to the cluster.

Explanation: Sets up the EKS cluster and node groups, allowing you to deploy applications on Kubernetes.

 4. **IAM Roles and Policies**

Files:
- iam_roles.tf

Steps:
- Create IAM roles and policies: Grants necessary permissions for EKS and nodes.

Explanation: Creates IAM roles and attaches policies, ensuring secure and appropriate access.

 Commands used:

1. terraform init: Initializes Terraform, downloading necessary providers and modules.

2. terraform apply: Applies the Terraform configuration, creating the infrastructure on AWS


5. **Deploy Application Components**

Files:
- kubernetes/frontend-deployment.yaml
- kubernetes/backend-deployment.yaml
- kubernetes/frontend-service.yaml

Steps:
- Deploy frontend and backend applications**: Defines how the applications are run on the cluster.
- Create a service for the frontend**: Exposes the frontend to external traffic.

Commands:

kubectl apply -f kubernetes/frontend-deployment.yaml
kubectl apply -f kubernetes/backend-deployment.yaml
kubectl apply -f kubernetes/frontend-service.yaml
```
Explanation: 
- kubectl apply -f kubernetes/frontend-deployment.yaml: Deploys the frontend application.
- `kubectl apply -f kubernetes/backend-deployment.yaml: Deploys the backend application.
- `kubectl apply -f kubernetes/frontend-service.yaml: Exposes the frontend application to external traffic.

6. ### Setup Prometheus and Grafana with Helm

1. Install Helm

(https://helm.sh/docs/intro/install/).

2. Add Helm Repositories

Add the necessary Helm repositories for Prometheus and Grafana.

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update

Explanation: 
- Adds the Helm chart repositories for Prometheus and Grafana, allowing you to install their charts.

3. Install Prometheus

Use Helm to install Prometheus.

helm install prometheus prometheus-community/prometheus

Explanation: 
- Deploys Prometheus on your Kubernetes cluster using the default configurations provided by the Helm chart.

4. Install Grafana

Use Helm to install Grafana.
helm install grafana grafana/grafana
Explanation: 
- Deploys Grafana on your Kubernetes cluster using the default configurations provided by the Helm chart.

*** Commands for Verification and Access

a) Check the status of the Prometheus and Grafana pods

kubectl get pods
Explanation: 
- Lists all the pods in your Kubernetes cluster, including those for Prometheus and Grafana, allowing you to verify their status.

b) Retrieve the Prometheus and Grafana service URLs

kubectl get svc -l app=prometheus -o jsonpath="{.items[0].status.loadBalancer.ingress[0].hostname}"
kubectl get svc -l app.kubernetes.io/name=grafana -o jsonpath="{.items[0].status.loadBalancer.ingress[0].hostname}"

Explanation: 
- Retrieves the external URLs for accessing Prometheus and Grafana services.

c) Summary of Commands:

1. Add Helm Repositories:
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo add grafana https://grafana.github.io/helm-charts
    helm repo update

2. Install Prometheus:
    helm install prometheus prometheus-community/prometheus

3. Install Grafana:
    helm install grafana grafana/grafana

4. Verify the Installations:
       kubectl get pods


### Explanation of Commands:

- helm repo add: Adds the Helm chart repositories for Prometheus and Grafana.
- helm repo update: Updates the local Helm chart repository cache.
- helm install: Installs Prometheus and Grafana using their respective Helm charts.
- kubectl get pods: Verifies the status of the deployed pods.
- kubectl get svc: Retrieves the external URLs for the Prometheus and Grafana services.

This setup should cover the necessary steps to deploy Prometheus and Grafana using Helm, providing monitoring and visualization capabilities for your Kubernetes cluster.


                

                    ***WHY I CHOOSEN THIS APPROCH***

1. Terraform for Infrastructure Setup

Reason: 
- Infrastructure as Code (IaC): Terraform allows defining infrastructure in code, which can be versioned, shared, and reused. This makes infrastructure management consistent and repeatable.
- Declarative Configuration: Terraform's declarative language allows you to define the desired state of your infrastructure, and Terraform takes care of achieving that state.
- AWS Integration: Terraform has strong support for AWS resources, making it suitable for creating and managing AWS infrastructure.

 2. VPC and Subnets Configuration
Reason:
- Network Isolation: Creating a VPC provides an isolated network environment for your resources, improving security and management.
- Subnet Segmentation: Using both public and private subnets allows for better control over network access. Public subnets can host resources that need internet access, while private subnets can host internal resources that should not be directly accessible from the internet.

 3. EKS Cluster and Node Groups
Reason:
- Managed Kubernetes: AWS EKS (Elastic Kubernetes Service) provides a managed Kubernetes service, reducing the operational overhead of maintaining the Kubernetes control plane.
- Scalability: EKS allows for easy scaling of the Kubernetes cluster by adding or removing node groups as needed.
- Integration with AWS Services: EKS integrates well with other AWS services, such as IAM for access control and CloudWatch for monitoring.

 4. IAM Roles and Policies
Reason:
- Fine-Grained Access Control: IAM roles and policies provide fine-grained access control, ensuring that resources and services have the minimum required permissions.
- Security: Using IAM roles and policies ensures that the security principles of least privilege are followed, reducing the risk of unauthorized access.

 5. Kubernetes for Application Deployment
Reason:
- Container Orchestration: Kubernetes excels at orchestrating containerized applications, managing their deployment, scaling, and operations.
- Resource Management: Kubernetes allows for fine-grained resource management, ensuring that applications have the necessary resources without over-provisioning.
- Resilience and Self-Healing: Kubernetes automatically handles failed containers, restarting them to ensure high availability.

 6. Horizontal Pod Autoscaling (HPA) with Kubernetes
Reason:
- Dynamic Scaling: HPA automatically scales the number of pod replicas based on CPU utilization, ensuring that the application can handle varying loads efficiently.
- Resource Optimization: By scaling pods up or down based on demand, HPA helps optimize resource usage and cost.

 7. Monitoring with Prometheus and Grafana
Reason:
- Comprehensive Monitoring: Prometheus provides powerful metrics collection and querying capabilities, essential for monitoring the health and performance of the Kubernetes cluster and applications.
- Visualization: Grafana allows for creating detailed and customizable dashboards, providing clear visual insights into the metrics collected by Prometheus.
- Community Support: Both Prometheus and Grafana have strong community support and are widely adopted in the industry, ensuring ongoing updates and improvements.

 8. Using Helm for Prometheus and Grafana Deployment
Reason:
- Simplified Deployment: Helm charts simplify the deployment of complex applications like Prometheus and Grafana, handling the necessary Kubernetes resources and configurations.
- Flexibility and Customization: Helm allows for easy customization of the deployments through values files, enabling configurations tailored to specific needs.

Conclusion:

This approach ensures a robust, scalable, and manageable Kubernetes deployment on AWS. By leveraging Terraform for infrastructure setup and Kubernetes for application orchestration, along with Prometheus and Grafana for monitoring, the solution is designed to meet the requirements of modern cloud-native applications, ensuring high availability, scalability, and observability.
