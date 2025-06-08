# 🚀 silence-k8s

> Kubernetes deployment of the [`ofirgaash/web`](https://github.com/ofirgaash1/silence-k8s/tree/main/app) static app, powered by modern DevOps tooling.

---

## 🔧 Stack

- 🐳 Docker + ECR – containerized build, pushed to AWS Elastic Container Registry  
- ⛵ Helm – declarative deployment via reusable Helm charts  
- ☸️ EKS – running on Amazon Elastic Kubernetes Service  
- 🚦 Traefik – ingress controller for routing and TLS termination  
- 📈 Prometheus + Grafana – monitoring stack via `kube-prometheus-stack`  
- 🤖 Jenkins – CI/CD pipeline with GitHub as source control

---

## 🌐 Live URLs

| Component  | URL |
|------------|-----|
| 🧩 App     | [http://silence.ofirgaash.click/](http://silence.ofirgaash.click/) |
| 📊 Grafana | [http://grafana.ofirgaash.click/](http://grafana.ofirgaash.click/?orgId=1&from=now-6h&to=now&timezone=browser) |
| 🔁 Jenkins | [Jenkins Job](http://vpn.aws.cts.care/job/ofir-EKS-deployment/) |
| 📦 ECR     | [ECR Repository](https://il-central-1.console.aws.amazon.com/ecr/repositories/private/314525640319/ofirgaash/web?region=il-central-1) |

---

## 🛠️ Quick Overview

How the deployment works — summarized:

1. **App code** lives in `silence-k8s/app`, built using Docker.
2. Image is tagged and pushed to [ECR](https://il-central-1.console.aws.amazon.com/ecr).
3. A **Helm chart** (`silence-k8s/charts/silence-web`) defines the Kubernetes resources.
4. Jenkins job builds the image and runs `helm upgrade` to deploy it.
5. **Ingress** handled by Traefik, with a DNS CNAME pointing to the AWS ELB.
6. Monitoring is done via the **Prometheus Operator**, with Grafana dashboards.

---

## 🧪 Dev Tips

To test locally:

```bash
# Forward Grafana to localhost
kubectl -n monitoring-ofir port-forward svc/kube-prometheus-grafana 3000:80

# Forward the app
kubectl -n ofirgaash port-forward svc/silence-web 8080:80
