<!---:lai-name: BigQuery--->
<div align="center">
<img src="static/big-query-icon.png" width="200px">

    A Lightning component to run queries on BigQuery.
    ______________________________________________________________________

![Tests](https://github.com/PyTorchLightning/LAI-bigquery/actions/workflows/ci-testing.yml/badge.svg)
</div>

### About

This component lets you run queries against a BigQuery warehouse.

### Use the component

To Run a query

```python
import pickle

import lightning as L
from lightning_bigquery import BigQuery

class ReadResults(L.LightningWork):
    def run(self, result_filepath):
        with open(result_filepath, "rb") as _file:
            data = pickle.load(_file)

        # Print top results from the dataframe
        print(data.head())

class GetHackerNewsArticles(L.LightningFlow):
    def __init__(self, project, location):
        super().__init__()
        self.bq_client = BigQuery(project=project, location=location)
        self.reader = ReadResults()

    def run(self):
        query = """select title, score from `bigquery-public-data.hacker_news.stories` limit 20"""

        self.bq_client.query(query, to_dataframe=True)

        if self.bq_client.has_succeeded:
            # The results from the query are saved as a pickled file.
            self.reader.run(self.bq_client.result_path)

app = L.LightningApp(GetHackerNewsArticles(project="grid-data-prod", location="US"), debug=True)
```

### Install
Run the following to install:
```shell
git clone https://github.com/PyTorchLightning/google-cloud.git
cd LAI-bigquery
pip install -r requirements.txt
pip install -e .
```
