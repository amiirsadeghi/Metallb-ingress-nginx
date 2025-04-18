
# Metallb + Ingress NGINX Setup

## Project Tree

```bash
.
├── metallb
│   ├── metallb-native.yaml
│   ├── metallb-ippool.yaml
│   └── metallb-L2Advertisement.yaml
├── ingress-nginx
│   └── deploy.yaml
├── apps
│   └── demo-app.yaml
└── README.md
```

## Scenario Overview

In this project, we demonstrate how to set up a bare-metal Kubernetes cluster with **MetalLB** as a software load balancer and **NGINX Ingress Controller** to handle HTTP routing. This setup allows external clients to access multiple services via a single Virtual IP (VIP) provided by MetalLB and routed by NGINX based on host headers.

### Key Components
- **MetalLB (Native Mode)**: Provides external load balancing by assigning VIPs to Kubernetes services.
- **NGINX Ingress Controller**: Routes HTTP traffic to backend services using host and path rules.
- **Demo Application**: A simple HTTP echo service defined in `apps/demo-app.yaml`, exposed via Ingress.

## Files 📄 Description

### `metallb/metallb-native.yaml`
Deploys MetalLB in native mode. MetalLB uses this manifest to create its controller and speaker components.

```bash
kubectl apply -f metallb/metallb-native.yaml
```

### `metallb/metallb-ippool.yaml`
Defines the IP address pool from which MetalLB will allocate VIPs.

```yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: metallb-pool
  namespace: metallb-system
spec:
  addresses:
    - 192.168.32.51-192.168.32.100
  autoAssign: true
```

Apply with:

```bash
kubectl apply -f metallb/metallb-ippool.yaml
```

### `metallb/metallb-L2Advertisement.yaml`
Configures Layer 2 advertisement, allowing MetalLB to announce the VIPs on the local network.

```bash
kubectl apply -f metallb/metallb-L2Advertisement.yaml
```

### `ingress-nginx/deploy.yaml`
Contains the Deployment and Service for the NGINX Ingress Controller with `type: LoadBalancer`, enabling MetalLB to assign a VIP.

```bash
kubectl apply -f ingress-nginx/deploy.yaml
```

### `apps/demo-app.yaml`
Defines a demo application (Deployment, Service, and Ingress) that responds with a simple message. The Ingress resource maps `web.local` to this service.

```bash
kubectl apply -f apps/demo-app.yaml
```

## Step-by-Step Setup

1. **Install MetalLB (Native Mode)**
   ```bash
   kubectl apply -f metallb/metallb-native.yaml
   ```

2. **Configure IP Pool**
   ```bash
   kubectl apply -f metallb/metallb-ippool.yaml
   ```

3. **Enable L2 Advertisement**
   ```bash
   kubectl apply -f metallb/metallb-L2Advertisement.yaml
   ```

4. **Deploy NGINX Ingress Controller**
   ```bash
   kubectl apply -f ingress-nginx/deploy.yaml
   ```

5. **Deploy the Demo Application**
   ```bash
   kubectl apply -f apps/demo-app.yaml
   ```

6. **Test the Application**
   - Retrieve the VIP assigned to the Ingress Controller:
     ```bash
     kubectl get svc -n ingress-nginx
     ```
   - Map the hostname in your `/etc/hosts` (or Windows hosts file):
     ```
     <VIP> web.local
     ```
   - Test with curl or browser:
     ```bash
     curl http://web.local
     ```

   You should see the demo application's response (e.g., "hello").

## Conclusion

This repository provides a complete example of leveraging **MetalLB** and **NGINX Ingress Controller** to expose services in a bare-metal Kubernetes cluster. You can extend this setup by adding more services and Ingress rules under the `apps/` directory.

---

Feel free to raise issues or contribute improvements!

