# Cronjob Chart
Setup Cronjob(s) and provides some examples.

## Installation
Using Helm install the chart using the below example.
```bash
helm upgrade --install cronjobs . \
    --namespace cron-jobs \
    --create-namespace \
    --atomic
```
