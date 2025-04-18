# 🚀 Metallb-ingress-nginx

This repository contains the necessary YAML files to install and configure [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/) with [MetalLB](https://metallb.universe.tf/) in a Kubernetes cluster. It is ideal for **on-premises (bare-metal)** environments where cloud load balancers are not available.

---

## 🧰 Prerequisites

- A running Kubernetes cluster (v1.13+ recommended)
- `kubectl` configured to access your cluster
- A free range of IP addresses on your local network (for MetalLB)

---

## 📁 Project Structure

```text
.
├── README.md 📘
├── apps 📦
│   └── demo-app.yaml              # Sample demo app (Deployment, Service, Ingress)
├── ingress-nginx 🚦
│   └── deploy.yaml                # NGINX Ingress Controller deployment
├── metallb 🧭
    ├── metallb-native.yaml       # Deploy MetalLB in native mode
    ├── metallb-ippool.yaml       # IP address pool definition
    └── metallb-L2Advertisement.yaml # L2 Advertisement configuration
```

---

## 🔧 Installation Steps

### ✅ 1. Install MetalLB

Apply MetalLB in native mode:

```bash
kubectl apply -f metallb/metallb-native.yaml
```

Apply the IP address pool configuration:

```bash
kubectl apply -f metallb/metallb-ippool.yaml
```

Apply the L2 Advertisement configuration:

```bash
kubectl apply -f metallb/metallb-L2Advertisement.yaml
```

### ✅ 2. Deploy NGINX Ingress Controller

```bash
kubectl apply -f ingress-nginx/deploy.yaml
```

Check the pods in the `ingress-nginx` namespace:

```bash
kubectl get pods -n ingress-nginx
```

### ✅ 3. Deploy the Test Application

```bash
kubectl apply -f apps/demo-app.yaml
```

Check the service exposed by NGINX Ingress Controller:

```bash
kubectl get svc -n ingress-nginx
```

Once an external IP is assigned by MetalLB, you can test your app by adding this line to `/etc/hosts`:

```bash
<external-ip>  web.local
```

Then open your browser and visit: [http://web.local](http://web.local)

---

## 🧠 Why use MetalLB with Ingress?

- MetalLB enables LoadBalancer services in bare-metal clusters 🌐
- NGINX Ingress Controller helps route traffic based on host/path headers
- You avoid exposing every service with a separate IP, using just one VIP

---

## 📚 Resources

- [NGINX Ingress Controller Docs](https://kubernetes.github.io/ingress-nginx/)
- [MetalLB Documentation](https://metallb.universe.tf/)
- [Kubernetes Ingress Concepts](https://kubernetes.io/docs/concepts/services-networking/ingress/)
