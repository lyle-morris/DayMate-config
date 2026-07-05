# DayPal Analytics API

Small Google Cloud Run service for the DayPal analytics dashboard.

It reads aggregated GA4 reporting data from property `544281382` and returns only summary metrics. It does not collect, store, or expose ZIP/postal code, city, GPS coordinates, health data, weather values, or personal information.

## Endpoints

```text
GET /health
GET /api/daypal-analytics
```

## Environment variables

```text
GA4_PROPERTY_ID=544281382
ALLOWED_ORIGIN=https://lyle-morris.github.io
CACHE_TTL_MS=900000
```

## Deployment guardrails

Use Cloud Run request-based billing with:

```text
min instances = 0
max instances = 1
memory = 256Mi
cpu = 1
```

Add a small billing budget alert in Google Cloud, such as `$1`, before production use.

## Google Analytics access

The Cloud Run runtime service account needs access to the GA4 property.

In Google Analytics:

```text
Admin -> Property access management -> Add user
```

Add the service account email with Viewer access.

## Deploy from local Google Cloud CLI

From this folder:

```bash
gcloud config set project project-631610e4-a97f-4d87-923
gcloud services enable run.googleapis.com cloudbuild.googleapis.com analyticsdata.googleapis.com

gcloud run deploy daypal-analytics-api \
  --source . \
  --region us-east1 \
  --allow-unauthenticated \
  --min-instances 0 \
  --max-instances 1 \
  --memory 256Mi \
  --cpu 1 \
  --set-env-vars GA4_PROPERTY_ID=544281382,ALLOWED_ORIGIN=https://lyle-morris.github.io,CACHE_TTL_MS=900000
```

After deployment, copy the Cloud Run service URL and set it in `analytics.html` as the dashboard API URL.
