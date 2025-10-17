---
slug: deploy-nginx-via-rancher
id: sj8vg2wy93kq
type: challenge
title: Deploy Nginx with Editor, Terminal & Rancher GUI
teaser: Author YAML in the Editor, apply from the Terminal, confirm in the Rancher
  GUI.
tabs:
- id: sym3n7uxthsu
  title: Terminal
  type: terminal
  hostname: docker-rancher
- id: xeksunkqynky
  title: Editor
  type: code
  hostname: docker-rancher
  path: /root
- id: rhzg3t6ky16d
  title: Rancher GUI
  type: service
  hostname: docker-rancher
  path: /
  port: 8443
difficulty: intermediate
timelimit: 900
enhanced_loading: true
---

# Deploy Nginx (Editor → Terminal → GUI)

In this challenge you’ll:
1) **Write** the Kubernetes manifests in the **Editor**
2) **Apply** them with `kubectl` from the **Terminal** (into the Rancher container)
3) **Verify** the result in the **Rancher GUI**

---

## 1) Log into Rancher (GUI tab)

Open the **Rancher GUI** tab and log in:

- **User:** `admin`
- **Password:** `cat /root/.rancher_admin_password` (from the Terminal)

Navigate to **local → Workloads** (we’ll return here in step 4).

---

## 2) Author the manifests (Editor tab)

Create `/root/nginx-deploy.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  namespace: default
spec:
  replicas: 1
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
        image: nginx:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: default
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
  type: NodePort
```
Save the file.

---

## 3) Apply from the Terminal

We’ll pipe the YAML into `kubectl` inside the Rancher container so you can keep editing locally in the Editor and apply instantly.

```bash
# Apply using stdin to the rancher container
cat /root/nginx-deploy.yaml \
  | docker exec -i rancher sh -c 'kubectl apply -f -'

# Verify pods
docker exec rancher sh -c 'kubectl get pods -n default -l app=nginx'
```

You should see the pod in **Running** state.

## 4) Confirm in Rancher GUI

Switch back to **Rancher GUI → local → Workloads → Deployments**
You should see **nginx-deploy** healthy.
Then go to **Service Discovery → Services** and confirm **nginx-service** exists.
