# Federated Analysis Example

An example for performing federated analysis in the simulation environment.

1. Once the `compute_metrics.ipynb` has run completely and produced a `metrics.csv` file, we can proceed with the federated learning component of the example.

2. Confirm the test federated learning server is up and running by contacting your relevant AusCAT representative. Confirm the IP address of the example federated learning server and the open port to communicate with it

3. Create a new stack through your example client's portainer or edit your existing auscat stack that contains the other AusCAT services, to setup the example federated learning client. The following can be used as a template:

```yaml
    fed_analysis_client_1:
    image: auscat/fed_analysis_flwr:client
    environment:
        DATAFILE_CSV: /mnt/working/metrics.csv
        FEATURES_LIST: mean,D0,D10,D20,D30
        SERVER_HOSTNAME: 203.101.225.47
        SERVER_PORT: 8443
    volumes:
    - analysis-data:/mnt
    restart: "on-failure"
```

Note:
- DATAFILE_CSV: is the path to the `metrics.csv` file in the container, after being mapped from the docker volume
- FEATURES_LIST: comma-spearated list of features from the DATAFILE_CSV that we wish to aggregate in the federated analysis.
    Please note that this must be identical across all clients, with preserved ordering
- SERVER_HOSTNAME: IP address of the machine hosting the fedearted learning server
- SERVER_PORT: the port on which the federated learning server us running on, on the server host machine
- For volumes: map the `analysis-data` volume to a path in the container, under which the DATAFILE_CSV will sit

4. Deploy the new (or update the existing) stack. Observe the logs of the container to see the outcome of the federated learning task.

5. Great! you have now successfully run a federated learning task (potentially in collaboration with other test clients) and have setup a fully-functional AusCAT simulation environment.
