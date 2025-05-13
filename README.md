# kustomize-with-AWS
Here’s a **clear, broken-down roadmap** of your **Kustomize mini project** titled *"Introduction to Configuration Management in Kubernetes with Kustomize"*. It shows **what to do** and **where to do it** step-by-step.

---

### **PHASE 1: Setup and Environment Preparation**

#### **Step 1: Install Kustomize**

* **Where**: On your **local machine** (Linux/Mac/Windows)
* **What**:

  * Download Kustomize from [GitHub releases](https://github.com/kubernetes-sigs/kustomize/releases).
  * Extract and move the binary to your system PATH (`/usr/local/bin` on Linux).
  * Run `kustomize version` to confirm installation.

#### **Step 2: Install Minikube (Local Kubernetes Cluster)**

* **Where**: On your **local machine**
* **What**:

  * Install Minikube from: [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)
  * Start the cluster with: `minikube start`
  * Verify it with: `kubectl get nodes`

---

### **PHASE 2: Learn Kustomize Core Concepts**

#### **Step 3: Create Directory Structure**

* **Where**: Your **project folder** (e.g., `~/projects/kustomize-demo`)
* **What**:

```bash
mkdir -p myapp/base
mkdir -p myapp/overlays/dev
mkdir -p myapp/overlays/prod
```

#### **Step 4: Create Base Deployment**

* **Where**: `myapp/base/deployment.yaml`
* **What**: Write a simple Deployment manifest:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

#### **Step 5: Create Base `kustomization.yaml`**

* **Where**: `myapp/base/kustomization.yaml`
* **What**:

```yaml
resources:
  - deployment.yaml
```

---

### **PHASE 3: Create Overlays for Customization**

#### **Step 6: Dev Overlay Patch**

* **Where**: `myapp/overlays/dev/replica_count.yaml`
* **What**:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
```

#### **Step 7: Dev `kustomization.yaml`**

* **Where**: `myapp/overlays/dev/kustomization.yaml`
* **What**:

```yaml
resources:
  - ../../base
patchesStrategicMerge:
  - replica_count.yaml
```

---

### **PHASE 4: Deploy to Local Kubernetes (Minikube)**

#### **Step 8: Deploy with Kustomize**

* **Where**: From the **root project directory**
* **What**:

```bash
kubectl apply -k myapp/overlays/dev/
```

#### **Step 9: Verify Deployment**

* **Where**: Terminal
* **What**:

```bash
kubectl get all
```

---

### **PHASE 5: Deploy to AWS (Optional Advanced Task)**

#### **Step 10: Setup AWS CLI**

* **Where**: Local machine
* **What**:

```bash
aws configure
```

#### **Step 11: Install `eksctl`**

* **Where**: Local machine
* **What**:

  * Install from: [https://eksctl.io/introduction/#installation](https://eksctl.io/introduction/#installation)

#### **Step 12: Create EKS Cluster**

* **Where**: Terminal
* **What**:

```bash
eksctl create cluster --name my-kustomize-cluster --version 1.21 --region us-west-2 --nodegroup-name my-nodes --node-type t2.medium
```

#### **Step 13: Deploy to EKS**

* **Where**: From your project root
* **What**:

```bash
kubectl apply -k myapp/overlays/dev/
```

#### **Step 14: Verify**

```bash
kubectl get all
```

#### **Step 15: Cleanup (To Avoid AWS Charges)**

```bash
kubectl delete -k myapp/overlays/dev/
eksctl delete cluster --name my-kustomize-cluster
```

---

Would you like me to generate the YAML files and folder structure for you as a zip, or would you prefer the bash script to automate setup?
