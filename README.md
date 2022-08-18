# Anthos Demo for Cloud Next 2022

## Getting started

1. Clone this repo.

   ```bash
   git clone https://github.com/ameer00/anthosnext22.git
   cd anthosnext22
   ```

1. Create a GCP project and assign a billing account.

   ```bash
   gcloud projects create <PROJECT_NAME> --org <ORG_ID>
   gcloud beta billing projects link <PROJECT_ID> --billing-account <BILLING_ACCOUNT>
   ```

1. Define your GCP project ID.

   ```bash
   export PROJECT_ID=<YOUR PROJECT ID>
   ```

1. Set variables.

   ```bash
   export GKE1=gke1-r1a-prod
   export GKE1_LOCATION=us-central1-a
   export GKE2=gke2-r1b-prod
   export GKE2_LOCATION=us-central1-b
   export GKE3=gke3-r2a-prod
   export GKE3_LOCATION=us-west2-a
   export GKE4=gke4-r2b-prod
   export GKE4_LOCATION=us-west2-b
   export GKE5=gke5-config
   export GKE5_LOCATION=us-west1-a
   export KCC_REGION=us-central1
   ```

1. Configure permissions for Cloud Build.

   ```bash
   gcloud config set core/project ${PROJECT_ID}
   export PROJECT_NUMBER=$(gcloud projects describe ${PROJECT_ID} --format 'value(projectNumber)')
   gcloud projects add-iam-policy-binding ${PROJECT_ID} --member serviceAccount:${PROJECT_NUMBER}@cloudbuild.gserviceaccount.com --role roles/owner
   ```

1. Build

   ```bash
   gcloud builds submit --substitutions=_PROJECT_ID=${PROJECT_ID}
   ```

1. View build in the Console.

   ```bash
   https://console.cloud.google.com/cloud-build
   ```

   > This step can take up to an hour to complete.

1. Connect to clusters.

   ```bash
   gcloud container clusters get-credentials ${GKE1} --zone ${GKE1_LOCATION} --project ${PROJECT_ID}
   gcloud container clusters get-credentials ${GKE2} --zone ${GKE2_LOCATION} --project ${PROJECT_ID}
   gcloud container clusters get-credentials ${GKE3} --zone ${GKE3_LOCATION} --project ${PROJECT_ID}
   gcloud container clusters get-credentials ${GKE4} --zone ${GKE4_LOCATION} --project ${PROJECT_ID}
   gcloud container clusters get-credentials ${GKE5} --zone ${GKE5_LOCATION} --project ${PROJECT_ID}
   gcloud anthos config controller get-credentials config-controller --location=${KCC_REGION} --project=${PROJECT_ID}
   kubectl config rename-context gke_${PROJECT_ID}_${GKE1_LOCATION}_${GKE1} ${GKE1}
   kubectl config rename-context gke_${PROJECT_ID}_${GKE2_LOCATION}_${GKE2} ${GKE2}
   kubectl config rename-context gke_${PROJECT_ID}_${GKE3_LOCATION}_${GKE3} ${GKE3}
   kubectl config rename-context gke_${PROJECT_ID}_${GKE4_LOCATION}_${GKE4} ${GKE4}
   kubectl config rename-context gke_${PROJECT_ID}_${GKE5_LOCATION}_${GKE5} ${GKE5}
   kubectl config rename-context gke_${PROJECT_ID}_${KCC_REGION}_krmapihost-config-controller kcc
   ```
