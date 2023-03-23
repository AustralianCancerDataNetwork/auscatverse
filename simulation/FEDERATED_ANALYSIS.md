# Federated Analysis Example

An example for performing federated analysis in the simulation environment.

1. Once the `compute_metrics.ipynb` has run completely and produced a `metrics.csv` file, we can proceed with the federated learning component of the example.

2. Confirm the test federated learning server is up and running by contacting your relevant AusCAT representative. Confirm the IP address of the example federated learning server and the open port to communicate with it

3. Create a new stack through your example client's portainer or edit your existing auscat stack that contains the other AusCAT services, to setup the example federated learning client. The following can be used as a template:

```yaml
    fed_analysis_client_1:
    image: auscat/fed_analysis_flwr:client
    environment:
        DATAFILE_CSV: /PATH/TO/BINDMOUNT/"working"/ON/CONTAINER/working/metrics.csv
        FEATURES_LIST: FEATURES,FROM,METRICS,CSV,SEPARATED,BY,COMMAS # Note this must be identical across all clients, with preserved ordering
        SERVER_HOSTNAME: FEDERATED_LEARNING_SERVER_IP_ADDRESS
        SERVER_PORT: FEDERATED_LEARNING_SERVER_RUNNING_PORT
    volumes:
    - analysis-data:/PATH/TO/BINDMOUNT/"working"/ON/CONTAINER/
    restart: "on-failure"
```

eg.

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

4. Deploy the new (or update the existing) stack. Observe the logs of the container to see the outcome of the federated learning task.

5. Great! you have now successfully run a federated learning task (potentially in collaboration with other test clients) and have setup a fully-functional AusCAT simulation environment.
