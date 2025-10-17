---
slug: deploy-rancher
id: ykqdm0hvxqhn
type: challenge
title: Deploy Rancher & Retrieve Admin Password
teaser: Rancher is auto-started for you; retrieve the bootstrap admin password.
tabs:
- id: rnahmeuxs1pn
  title: Terminal
  type: terminal
  hostname: docker-rancher
- id: nygtedypwh7s
  title: Editor
  type: code
  hostname: docker-rancher
  path: /root
- id: vf44xicq04rs
  title: Rancher GUI
  type: service
  hostname: docker-rancher
  path: /
  port: 8443
difficulty: basic
timelimit: 900
enhanced_loading: true
---

# Deploy Rancher & Retrieve Admin Password

Rancher is being started automatically by the environment.

## 1) Verify the container
```bash
docker ps | grep rancher
```

## 2) Retrieve and save the admin password
```bash
docker exec rancher sh -c \
  "kubectl get secret -n cattle-system bootstrap-secret \
     -o go-template='{{.data.bootstrapPassword|base64decode}}{{\"\n\"}}'" \
| tee /root/.rancher_admin_password
```

Click **Check** to validate. You'll use this password to log in on the next challenge.