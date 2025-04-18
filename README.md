
# Metallb + Ingress NGINX Setup

## Scenario Overview

In this project, we set up a Kubernetes cluster with MetalLB and the NGINX Ingress Controller to handle HTTP traffic and route it to backend services. The goal is to replicate the behavior of a load balancer using MetalLB to manage traffic between external clients and services running inside a Kubernetes cluster.

### Key Concepts:
- **MetalLB**: A software load balancer for Kubernetes that allows us to expose services on a virtual IP (VIP) that can be accessed externally.
- **NGINX Ingress Controller**: A component that handles HTTP(S) routing inside the cluster, using the host headers in HTTP requests to forward traffic to the appropriate services.

## Components of the Setup:
1. **MetalLB**: Provides external access to services within the Kubernetes cluster using a VIP.
2. **NGINX Ingress Controller**: Handles routing based on HTTP headers, allowing multiple services to share the same VIP.
3. **Test Application**: A simple HTTP application exposed via NGINX Ingress to demonstrate the setup.

### Project Steps:
1. **Install MetalLB**: MetalLB was installed to provide a load balancer functionality within the Kubernetes cluster.
2. **Install NGINX Ingress Controller**: The NGINX Ingress Controller was set up to manage routing to different services based on HTTP host headers.
3. **Create a Test Application**: A simple "Hello World" HTTP service was deployed in the cluster. This service is exposed via a Kubernetes service and accessible externally via NGINX Ingress.

---

## Files

### 1. `metallb-native.yaml`
This file installs the **MetalLB Native** load balancer, enabling external access to Kubernetes services. It includes the necessary configuration to deploy MetalLB using the **native mode**. In this mode, MetalLB uses a range of IP addresses to assign to services that need external access.

To install MetalLB, apply the following command:

```bash
kubectl apply -f metallb-native.yaml
```

This will create the necessary MetalLB components in your cluster.

### 2. `metallb-ippool.yaml`
This file defines an IP Address Pool for MetalLB to assign IPs from the range `192.168.32.51-192.168.32.100`. It also enables automatic IP assignment.

To apply the IP pool configuration:

```bash
kubectl apply -f metallb-ippool.yaml
```

### 3. `metallb-L2Advertisement.yaml`
Defines the L2Advertisement configuration for MetalLB, allowing the VIP to be advertised to the external network.

To apply the L2Advertisement configuration:

```bash
kubectl apply -f metallb-L2Advertisement.yaml
```

### 4. `demo-app.yaml`
This file contains the **Deployment**, **Service**, and **Ingress** resources for a simple HTTP application. The application is exposed using the NGINX Ingress Controller, which handles traffic routed through MetalLB's VIP.

To deploy the test application:

```bash
kubectl apply -f demo-app.yaml
```

### 5. `web-service.yaml`
This file defines a Kubernetes Service that exposes the web application on port 80 and forwards traffic to the container's port 5678.

### 6. `web-ingress.yaml`
This file defines an Ingress resource that uses the NGINX Ingress Controller to route traffic to the web service based on the hostname `web.local`.

---

## Step-by-Step Setup

### Step 1: Install MetalLB
1. Download the `metallb-native.yaml` file and apply it to your Kubernetes cluster:

    ```bash
    kubectl apply -f metallb-native.yaml
    ```

2. Apply the IP Address Pool configuration:

    ```bash
    kubectl apply -f metallb-ippool.yaml
    ```

3. Apply the L2Advertisement configuration:

    ```bash
    kubectl apply -f metallb-L2Advertisement.yaml
    ```

### Step 2: Install NGINX Ingress Controller
1. Deploy the NGINX Ingress Controller using the `ingress-nginx` official Helm chart or using a YAML manifest (if you haven't already).

    Example for installing via Helm:
    ```bash
    helm install nginx-ingress ingress-nginx/ingress-nginx
    ```

    Alternatively, you can apply the deployment YAML if you're using a manual approach.

### Step 3: Deploy Test Application
1. Apply the `demo-app.yaml` file to create the web service:

    ```bash
    kubectl apply -f demo-app.yaml
    ```

### Step 4: Test the Setup
1. Ensure that MetalLB has assigned a VIP and that it is accessible externally.
2. Access the test application using the VIP and the domain `web.local`.

---

## Conclusion

This setup demonstrates the power of combining MetalLB with NGINX Ingress Controller to provide load balancing and HTTP routing inside a Kubernetes cluster. By leveraging MetalLB, we were able to expose services externally on a Virtual IP (VIP), and using the NGINX Ingress Controller, we were able to route HTTP traffic based on the host header to different services within the cluster.

---

## GitHub Repository

You can find the project files on GitHub:

[Metallb-ingress-nginx Repository](https://github.com/amiirsadeghi/Metallb-ingress-nginx)
