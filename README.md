# Updating alertmanager configuration in k8s for slack notification

When you run a prometheus stack using operator, you cannot add or update the configuration files of alertmanager as it is being managed by operator. here we refer to official prometheus operator provided under helm chart:
 <https://prometheus-community.github.io/helm-charts>

so with steps to create the cluster and deploy prometheus are also provided

## create cluster in EKS/AKS/GKE

    eksctl create cluster
    (we are taking eks but you could change as you prefer)

### Deploy Prometheus Operator Stack

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    helm repo update
    kubectl create namespace monitoring
    helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
    helm ls

[Link to the chart: <https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack>]

### Check Prometheus Stack Pods

    kubectl get all -n monitoring

### Access Prometheus UI

    kubectl port-forward svc/monitoring-kube-prometheus-prometheus 9090:9090 -n monitoring &

### Access Alert manager UI

    kubectl port-forward -n monitoring svc/monitoring-kube-prometheus-alertmanager 9093:9093 &

### Notification Addition

Suppose we have two rules one for Pod crashing and other for high cpu load
We are using the api for alertmanager configuration to pass it to cluster that would be updated by operator

* remember to run in same namespace

#### create your slack space incomming webhook or slack api url

#### Reference

* <https://docs.openshift.com/container-platform/4.11/rest_api/monitoring_apis/alertmanagerconfig-monitoring-coreos-com-v1beta1.html>
