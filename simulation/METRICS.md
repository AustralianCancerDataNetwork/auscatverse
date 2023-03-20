# Metric Computation

An example Jupyter Notebook is provided which can be run to generate radiomic and dose metrics from the simulation environment data. Once the [simulation environment has been deployed](SIMULATION.md), this example demonstrates how to query the CatDB for a cohort of patients, fetch their imaging data from the Orthanc, convert this to a format more suited for our research tools (NIfTI), prepare visualisation of the data and finally compute first-order radiomic features as well as Dose Volume Histograms (DVHs) and some dose metrics. These are saved off to a CSV file, ready for [federated analysis](FEDERATED_ANALYSIS.md).

## Deploy Example Docker Image

The easiest way to run this example is to use the Docker Image which contains the example Jupyter Notebook and everything needed to run a Jupyter instance. Modify your Simulation Docker Stack in Portainer by adding the following under `services`:

```yaml
  analysis_example:
      image: 'auscat/etl:example'
      ports:
      - 8888:8888
      depends_on:
      - etl
      volumes:
      - analysis-data:/data
```

Also be sure to add the volume definition under `volumes:` at the end of the stack:

```yaml
  analysis-data:
```

Re-deploy the stack.

## Running the Example Notebook

Once the Docker image is up and running, you can access the Jupyter instance at: `http://[YOUR-HOST-IP]:8888`. You will be prompted for a password which has been set to a default of `password` (Note using this password is only suitable in a simulation environment).

Once logged in, you can navigate to `work` folder and find the `Compute_Metrics.ipynb` notebook. Open this Jupyter Notebook and follow the instructions within.

> Tip: You can quickly execute the cells within the Jupyter Notebook by pressing `Shift+Enter`.

## Analysing Metrics

The output of this example notebook is a file named `metrics.csv` which is stored in the `working` directory. This directory is mapped to the `analysis-data` volume we defined in the Docker stack above. Using this volume you can proceed with a [federated analysis](FEDERATED_ANALYSIS.md) using the metrics computed in this example.
