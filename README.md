### About

This component gives you the ability to interface with gcp.

### Use the component

Credentials can be provided in one of two ways:

1. Pass the credentials directly as a dictionary:

```python
import lightning as L
from lightning_gcp.bigquery import BigQueryWork


class GetData(L.LightningFlow):
    def __init__(self):
        super().__init__()
        self.client = BigQueryWork()

    def run(self, location, project, columns, dataset, table, credentials):
        query = f"""
            select
                {','.join(columns)}
            from
                `{dataset}.{table}`
        """
        self.client.run(query=query, project=project, location=location, credentials=credentials)
```

2. Add or create `~/.lighning.secrets/.secrets.json` with the following information with passing credentials in as a run parameter.

```json
{
  "google_service_account": {
    "type": "service_account",
    "project_id": "<PROJECT_ID>",
    "private_key_id": "<PRIVATE_KEY_ID>",
    "private_key": "<PRIVATE_KEY>",
    "client_email": "<CLIENT_EMAIL>",
    "client_id": "<CLIENT_ID>",
    "auth_uri": "https://accounts.google.com/o/oauth2/auth",
    "token_uri": "https://oauth2.googleapis.com/token",
    "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
    "client_x509_cert_url": "<CLIENT_CERT_URL>"
  }
}
```

### Install

```shell
git clone https://github.com/PyTorchLightning/google-cloud.git
cd google-cloud
pip install -e .
```
