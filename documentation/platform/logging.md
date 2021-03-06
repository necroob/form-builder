# Logging

Draft - include RAILS_LOG ENV var info

(Better still, turn it on by default in non-prod)

## Aggregated logs

Logs that have been exposed to the container they run in's stdout can be accessed via Kibana

https://kibana.cloud-platform.service.justice.gov.uk/_plugin/kibana

Forms can be identified by setting filtering by `kubernetes.container_name`, setting its value to that of the form you're after

## Raw logs

NB. [Stern](https://github.com/wercker/stern) is a logging tool that allows the viewing of logs from multiple pods at once - it's essentially a convenient shortcut equivalent to:

```
kubectl get pods -n $NAMESPACE | grep $FORM

kubectl logs -n $NAMESPACE $POD_ID_1
...
kubectl logs -n $NAMESPACE $POD_ID_N
```

## Form logs

`stern -n formbuilder-services-$ENVIRONMENT -l run=$FORM`

## Application logs

### Publisher

#### Web UI

`stern -n formbuilder-platform-$ENVIRONMENT -l app=fb-publisher-web-$ENVIRONMENT`

#### Workers

`stern -n formbuilder-platform-$ENVIRONMENT -l app=fb-publisher-workers-$ENVIRONMENT`

### User datastore

Due to the fact that the logs contain the user's data, they can only be viewed from within the app's pods.

Get the id of the pod you wish to inspect

`kubectl get pods -n formbuilder-platform-$ENVIRONMENT | grep fb-user-datastore`

Access the pod

`kubectl exec -ti -n formbuilder-platform-$ENVIRONMENT  $POD_ID -- bash`

View the logs

`cat /var/www/fb-user-datastore/log/production.log`

### Service token cache

Due to the fact that the logs contain service tokens and other sensitive information, they can only be viewed from within the app's pods.

- Get the id of the pod you wish to inspect

  `kubectl get pods -n formbuilder-platform-$ENVIRONMENT | grep fb-service-token-cache`

- Access the pod

  `kubectl exec -ti -n formbuilder-platform-$ENVIRONMENT  $POD_ID -- bash`

- View the logs

  `cat /var/www/fb-service-token-cache/log/production.log`

### Submitter

Due to the fact that the logs contain the user's data and other sensitive information, they can only be viewed from within the app's pods.

#### API

- Get the id of the pod you wish to inspect

  `kubectl get pods -n formbuilder-platform-$ENVIRONMENT | grep fb-submitter-api`

- Access the pod

  `kubectl exec -ti -n formbuilder-platform-$ENVIRONMENT  $POD_ID -- bash`

- View the logs

  `cat /var/www/fb-submitter/log/production.log`

#### Workers

- Get the id of the pod you wish to inspect

  `kubectl get pods -n formbuilder-platform-$ENVIRONMENT | grep fb-submitter-workers`

- Access the pod

  `kubectl exec -ti -n formbuilder-platform-$ENVIRONMENT  $POD_ID -- bash`

- View the logs

  `cat /var/www/fb-submitter/log/production.log`

## Metrics and alerts

Provided by Cloud Platform's Prometheus and Alertmanager instances

- [Grafana Dashboard](https://grafana.cloud-platform.service.justice.gov.uk/d/-hXgWMWWk/form-builder)
