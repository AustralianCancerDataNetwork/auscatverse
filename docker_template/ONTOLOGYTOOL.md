# Data harmonisation 

## D2RQ platform
D2RQ is a platform for mapping relational database data to RDF, which is a standard format used for representing and sharing data on the web. It is an open-source tool that can be used to create Linked Data from existing databases, enabling users to expose data on the web in a format that can be easily accessed and integrated with other data sources.
</br>
</br>
D2RQ works by creating a mapping between the relational database schema and the RDF model, which allows data to be automatically translated between the two formats. This allows users to create a virtual RDF graph that is dynamically generated from the database content.
</br>
</br>
In the AusCAT, the data were translated into a further standardized format using avaliable data dictionaries (ontologies) from the national Cancer Institue and others, e.g., the radiation oncology ontology (ROO) to create a findable accessible, interoperable and reusable (FAIR) data model.

### Docker image
Pull the Docker image **auscat/d2rq** from AusCAT DockerHub repository.

### Manage sensitive data with Docker secrets
The D2RQ service requires some RDF repository information. </br>
Click "Add secret", give it a name "rdf-lung.yaml" if you want to create a remote RDF repository called "lung", fill in the details, then click on "Create the secret":
```
rdfHost: rdf4j
rdfPort: 8080
rdfRepository: lung
dr2qFile: Sample_D2RQ.ttl
ontologyFile: ROO.0.5.1.owl
d2rLocation: /home/jovyan/d2rq
```

## RDF4j
RDF4J is an open-source Java-based framework for working with RDF data, which is a standard format for representing and sharing data on the web. It provides a set of APIs and tools for working with RDF data, including parsing, querying, and modifying RDF graphs.
</br>
</br>
In the AusCAT, RDF4J is used as an ontology tool to manage and query RDF data. It provides a flexible and efficient way to store and access large amounts of RDF data, enabling researchers and healthcare professionals to easily query and analyze data from different sources.

### Docker image
Pull the Docker image **auscat/rdf4j** from AusCAT DockerHub repository.

