# GSP1026
## Run in cloudshell
```cmd
export ZONE=
```
```cmd
gcloud beta container clusters create gmp-cluster --num-nodes=1 --zone $ZONE --enable-managed-prometheus
gcloud container clusters get-credentials gmp-cluster --zone=$ZONE
kubectl create ns gmp-test
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.2.3/examples/example-app.yaml
kubectl -n gmp-test apply -f https://raw.githubusercontent.com/GoogleCloudPlatform/prometheus-engine/v0.2.3/examples/pod-monitoring.yaml
git clone https://github.com/GoogleCloudPlatform/prometheus && cd prometheus
git checkout v2.28.1-gmp.4
wget https://storage.googleapis.com/kochasoft/gsp1026/prometheus
./prometheus \
--config.file=documentation/examples/prometheus.yml \
--export.label.project-id=$DEVSHELL_PROJECT_ID \
--export.label.location=$ZONE 
wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.3.1.linux-amd64.tar.gz
cd node_exporter-1.3.1.linux-amd64
cat > config.yaml <<EOF
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: node
    static_configs:
      - targets: ['localhost:9100']
EOF
gsutil mb -p $DEVSHELL_PROJECT_ID gs://$DEVSHELL_PROJECT_ID
gsutil cp config.yaml gs://$DEVSHELL_PROJECT_ID
gsutil -m acl set -R -a public-read gs://$DEVSHELL_PROJECT_ID
```
