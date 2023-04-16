# DICOM-RT anonymisation and PACS 

## Clinical Trial Processor (CTP)
### Quick reference
Clinical Trial Processor [here](https://mircwiki.rsna.org/index.php?title=MIRC_CTP_Articles)

### Docker image 
The Clinical Trial Processor (CTP) is an imaging research tool that is used in AusCAT. </br>
The CTP accepts incoming DICOM files for anonymisation before sending them to the Orthanc. </br>
CTP can be used to manage and move data between field centres and principal sites in multisite imaging clinical trials. </br>
This docker image consists of a customized pipeline that anonymizes DICOM objects based on Postgres key database and configures CTP to work with a Picture Archiving and Communication System (PACS) system Orthanc. </br>
Visit http://[IP_ADDRESS]:9090 in your web browser. Click login in the top right hand corner.

Pull the Docker image **auscat/customized_ctp** from AusCAT DockerHub repository.

### Manage sensitive data with Docker secrets
In portainer, under the “Secrets” section, create the sensitive variables that you would like to track securely for your docker stack. You need to add secrets before deploying the docker stack, secrets are encrypted during transit and at rest in a Docker Swarm:
</br>
1. From the menu select "Secrets" then click " + Add secret". 
2. Then you need to give the secret a descriptive name and write a definition of the secret in the "Secret" field. 3. Toggle "Encode secret" on if you want to encode the secret (useful when you use a plain-text password).
4. To set the superuser and superuser password for PostgresSQL Key Database, pass superuser sensitive information of the Key Database into Docker Swarm by creating **keydb_user** and **keydb_pass** secrets. 
</br>
</br>


## Orthanc 
Orthanc is an open-source PACS which stores the anonymised DICOM data. 
</br>
Orthanc provides a user-friendly web-based interface for managing and accessing medical imaging data, which can be accessed from any web-enabled device. The system includes various features such as search and filter tools, study comparison tools, and anonymization tools to protect patient privacy. </br>
Visit http://[IP_ADDRESS]:8042 in your web browser. Click ```All Patients``` and explore the anonymised simulation DICOM data imported. 

### Docker image 
Pull the Docker image **auscat/orthanc** from AusCAT DockerHub repository.

### Manage login credentials with Docker secrets
In portainer, under the “Secrets” section, create a secret called **"orthanc.json"**, copy/paste the contents below, replace the parts under "RegisteredUsers":
```
{
    "Name" : "ORTHANC in AusCAT",
    "RemoteAccessAllowed" : true,
    "AuthenticationEnabled" : true,
    "RegisteredUsers": {
        "admin":"admin"
    }
}
```

