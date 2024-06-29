# demo-io-minna
> [See today's slides here](https://docs.google.com/presentation/d/1vu6_5NwBIXlYVlimIE_ZvSqknvsIgUVqGBhPafRZIPI/edit?usp=sharing)
## Checklist
1. Configure your GCloud environment and account for Gemini
2. GKE configurations
3. Deployment to GKE
4. Log Analysis with Gemini
5. Build Worker Pool


### Task 1:
- Open Google Cloud Shell
- Execute the following commands
```bash
PROJECT_ID=$(gcloud config get-value project)
REGION=us-west1
echo "PROJECT_ID=${PROJECT_ID}"
echo "REGION=${REGION}"
```
- Let keep remembering the account details

```bash
USER=$(gcloud config get-value account 2> /dev/null)
echo "USER=${USER}"
```
- Activating required AI Companion Google Cloud APIs
```bash
gcloud services enable cloudaicompanion.googleapis.com --project ${PROJECT_ID}
```
- Iam Policy Gemini API

```bash
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/cloudaicompanion.user
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/serviceusage.serviceUsageViewer
```

### Task 2
- Enable GKE API
```bash
gcloud services enable container.googleapis.com --project ${PROJECT_ID}
```
- Setting up the right permissions
```bash
gcloud projects add-iam-policy-binding ${PROJECT_ID} --member user:${USER} --role=roles/container.admin
```
- Let's seek help from Gemini
> Since the Gemini/AI Companion has been enabled, Gemini icon will automatically popped up beside your cloud shell button at the top right corner of Google cloud console. If not, you can refresh.
  - Click the button to open Gemini Pane
  - Let's prompt it.
```bash
What's the gcloud command for creating a zonal GKE cluster with a custom number of nodes and custom machine type?
```
- Gemini response
```bash
gcloud container clusters create <CLUSTER_NAME> \
  --project=PROJECT_ID \
  --zone=COMPUTE_ZONE \
  --num-nodes=NUM_NODES \
  --machine-type=MACHINE_TYPE
```
- Let's insert the missing values
```bash
gcloud container clusters create test \
    --project=qwiklabs-gcp-01-5cebcc9fc0e3 \
    --zone=us-west1-c \
    --num-nodes=3 \
    --machine-type=e2-standard-4
```
### Task 3
- The system architecture
![image](https://github.com/Taiwrash/demo-io-minna/assets/49725691/f04daa88-de62-4cd6-a937-31fb76202c6e)
- Clone the repo
```bash
git clone --depth=1 https://github.com/GoogleCloudPlatform/microservices-demo
```
- Apply K8S config
```bash
  cd ~/microservices-demo
  kubectl apply -f ./release/kubernetes-manifests.yaml
```
- Deployment progress
```bash
kubectl get deployments
```
- Test server
```bash
echo "http://$(kubectl get service frontend-external -o=jsonpath='{.status.loadBalancer.ingress[0].ip}')"
```
### Task 4
- Open Logs Explorer on the GCP Console
- Request for query from Gemini
```bash
What is a Logs Explorer query to search for logs from Pods in a namespace called "default" in a GKE cluster named "test"?
```
- Response
```bash
resource.type="k8s_container"
resource.labels.cluster_name="test"
resource.labels.namespace_name="default"
```
- Paste in the query box and run the query
- Learning about logs: Click one of the return logs by the query above and you will see the button `Explain this log entry` that gives detail analysis of the log with the power Gemini

### Task 5
- Learn about worker pool within GCP Console
```bash
What is a Cloud Build worker pool?
```
- Block internet access
```bash
Can you create a private worker pool that has no access to the public internet?
```
- Let's create one with Gemini
```bash
What is the gcloud command for creating a private worker pool with no public egress?
```
- Response
```bash
gcloud builds worker-pools create POOL_NAME \
  --project=PROJECT_ID \
  --region=REGION \
  --no-public-egress
```
- Adapting the response
```bash
gcloud builds worker-pools create pool-test \
  --project=qwiklabs-gcp-01-5cebcc9fc0e3 \
  --region=us-west1 \
  --no-public-egress
```
- More detail from Gemini
```bash
How can I use gcloud to create a private Docker repository for container images in Artifact Registry?
```
- Adapting Gemini response

```bash
gcloud artifacts repositories create my-repo \
  --repository-format=docker \
  --location=us-west1 \
  --description="My private Docker repository"
```

## The End: Star this if looks helpful. Follow me on X and GitHub | [X:taiwrash](https://x.com/taiwrash) and on [GitHub:taiwrash](https://github.com/Taiwrash)
