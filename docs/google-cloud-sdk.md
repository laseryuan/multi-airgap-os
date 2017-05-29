# Get the cloud sdk image:
docker pull google/cloud-sdk

# Auth & save the credentials in gcloud-config volumes:
docker run -t -i --name gcloud-config google/cloud-sdk gcloud init

# If you would like to use service account instead please look here:
docker run -t -i --name gcloud-config google/cloud-sdk gcloud auth activate-service-account <your-service-account-email> --key-file /tmp/your-key.p12 --project <your-project-id>

# Re-use the credentials from gcloud-config volumes & run sdk commands:
docker run --rm -ti --volumes-from gcloud-config google/cloud-sdk gcloud info
docker run --rm -ti --volumes-from gcloud-config google/cloud-sdk gcloud components list
docker run --rm -ti --volumes-from gcloud-config google/cloud-sdk gcloud compute instances list
docker run --rm -ti --volumes-from gcloud-config google/cloud-sdk gsutil ls
