## Prometheus Monitoring

[Prometheus](https://github.com/prometheus) is an open-source systems monitoring and alerting toolkit originally built at [SoundCloud](https://soundcloud.com/).

Official site: [https://prometheus.io/](https://prometheus.io/)

### Architecture

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/win/20220415155904.png)

### How to use Prometheus

#### 1. With Helm

[https://helm.sh/docs/howto/charts\_tips\_and\_tricks/](https://helm.sh/docs/howto/charts\_tips\_and\_tricks/)

1. create Minikube cluster at first

```shell
minikube start
```

1. Install helm

```shell
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

1. Add repository of stable charts

```shell
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable "https://charts.helm.sh/stable"
helm repo update
```

1. check the repo

```shell
helm search repo kube-prometheus-stack
```

1. install chart

```shell
helm install prometheus prometheus-community/kube-prometheus-stack
```

1. port forward

```shell
kubectl port-forward deployment/prometheus-grafana 3000
```

1. In the browser

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220419165633.png)

1. find username and password

Here I choose the default username and password

`username: admin`

`password: prom-operator`

#### 2. With yaml file

[https://www.youtube.com/watch?v=YDtuwlNTzRc](https://www.youtube.com/watch?v=YDtuwlNTzRc)

[https://github.com/prometheus-operator/kube-prometheus#quickstart](https://github.com/prometheus-operator/kube-prometheus#quickstart)

* Setup

```shell
kubectl apply --server-side -f manifests/setup
kubectl apply -f manifests/
```

* open grafana

```shell
kubectl --namespace monitoring port-forward svc/grafana 3000
```

### References

[https://prometheus.io/docs/introduction/overview/](https://prometheus.io/docs/introduction/overview/)

[https://www.youtube.com/watch?v=h4Sl21AKiDg](https://www.youtube.com/watch?v=h4Sl21AKiDg)

[https://www.youtube.com/watch?v=QoDqxm7ybLc](https://www.youtube.com/watch?v=QoDqxm7ybLc)

[https://computingforgeeks.com/install-prometheus-server-on-debian-ubuntu-linux/](https://computingforgeeks.com/install-prometheus-server-on-debian-ubuntu-linux/)

[https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/prometheus-exporter/install-prometheus-commands.md](https://gitlab.com/nanuchi/youtube-tutorial-series/-/blob/master/prometheus-exporter/install-prometheus-commands.md)

[https://github.com/helm/charts/tree/master/stable/prometheus-operator](https://github.com/helm/charts/tree/master/stable/prometheus-operator)

[https://www.containiq.com/post/prometheus-alertmanager](https://www.containiq.com/post/prometheus-alertmanager)
