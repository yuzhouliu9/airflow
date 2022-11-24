# Learning apache airflow

References:
- https://www.youtube.com/watch?v=GDOw8ByzMyY
- https://www.youtube.com/watch?v=IH1-0hwFZRQ
- https://airflow.apache.org/docs/helm-chart/stable/manage-dags-files.html
- https://www.youtube.com/watch?v=4rQSa2zEWfw&list=PLYizQ5FvN6pvIOcOd6dFZu3lQqc6zBGp2&index=5


## Deploy airflow via helm chart
Start up minikube

Deploy airflow helm chart. The last step takes a few minutes.
```
kubectl create namespace airflow

helm repo add apache-airflow https://airflow.apache.org
helm repo update
helm install airflow apache-airflow/airflow --namespace airflow --debug
```

Forward the port
```
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow
```

Go to localhost:8080 to access UI. admin/admin.

## Adding own DAGs
The `values.yaml` file in this repo configures airflow to sync this git repo `dags` folder onto a PVC attached to the running airflow instance. You can then see these workflows in the UI. See `dags:` section in values.yaml.

Clone this repo. Update airflow helm chart with new values. Redo port forward, and localhost:8080 UI should have the DAGs from the dags folder of this repo.
```
helm upgrade --install airflow apache-airflow/airflow  -f values.yaml
kubectl port-forward svc/airflow-webserver 8080:8080 -n airflow
```
