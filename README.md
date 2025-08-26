# [LibSQL](https://github.com/tursodatabase/libsql) in Cloud Run:  Serverless SQLite over http

This repo gives you instructions and environment variables required to deploy LibSQL to Cloud Run in 'bottomless mode',
with the DB streaming its WAL to GCS in S3 Compatibility Mode.

### Prereqs:  Enable S3 Compatibility in GCS

* In the Google Cloud Console, go to Cloud Storage > Settings.
* Select the Interoperability Tab: Click on the Interoperability tab (You may need to enable this if you haven't used it previously)
* Under the section "Access keys for service accounts," click CREATE A KEY FOR A SERVICE ACCOUNT.
* Select a Service Account: Choose the service account that your Cloud Run service will use (the Default compute engine SA by default)
* Generate and Save Credentials: Click CREATE KEY. You will be shown an Access ID and a Secret. This is the only time the secret will be displayed. Copy both values.

In .env:
- set AWS_ACCESS_KEY_ID to the Access ID.
- set AWS_SECRET_ACCESS_KEY to the secret.

Make sure that this SA has at least the storage.objectadmin on your chosen bucket.

### Configure .env 

See the included .env file and replace the other values relevant for your project. You need not choose a bucket that already exists, libsql creates one with the chosen name.

### Deploy to cloud run

You can build and push the libsql Docker container from https://github.com/tursodatabase/libsql to your own Artifact Registry repo using `gcloud builds submit -t [tag]`, 
but this takes a long time (~30 min at default build speed).  You can also use the public image at gcr.io/jmahood-demo/libsql:latest 

`gcloud beta run deploy libsql --image=gcr.io/jmahood-demo/libsql:latest --max=1 --min=0 --env-vars-file=.env --no-cpu-throttling`

Then use one of the client libraries or tools listed in https://github.com/tursodatabase/libsql/blob/main/README.md 
