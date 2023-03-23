```yaml
version: "3.8"
services:
  fed_analysis_server:
    image: auscat/fed_analysis_flwr:server
    environment:
      NUM_CLIENTS: 2
      SERVER_HOSTNAME: "[::]"
      SERVER_PORT: 8080
    restart: "on-failure"
    ports:
      - "8443:8080"
```
