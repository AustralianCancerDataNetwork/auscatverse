# A template containing sub-sections for documenting AusCAT centre-specific info
Should be stored in Word document under the Centre's channel in the AusCAT Team's channel

## Admin
### Contacts
* Contact personnel from the AusCAT team assigned to the centre and relevant contacts from the centre involved with the AusCAT project (technically, clinically, administratively). Preferrably a table of the following structure:

eg. 
| Contact      | Team   | Role                      | Email     |
| ---          | ---    | ---                       | ---       |
| John Smith   | AusCAT | Technical support/contact | js@js.com |
| Bobby Wilson | Site X | Medical Physicist         | bw@bw.com |
...

### Available Hardware
* The hardware that is avaliable for the AusCAT project at the site.  Preferrably a table of the following structure:

eg. 
| Machine host name             | IP Address    | Overall Setup Type        | Site IT Manager               | Specs                         | Contact Access                                            |
| ---                           | ---           | ---                       | ---                           | ---                           | ---                                                       |
| name1.domain.nswhealth.net    | 10.x.x.x      | On-Site Ubuntu VM         | On-Site CTC IT Team           | CPU, Memory, disk space, GPU  | Initials of tech contact who has access to the machine    |
| name2.domain.nswhealth.net    | 10.x.x.x      | Azure Cloud Ubuntu VM     | On-Site CTC IT Team           | CPU, Memory, disk space, GPU  | Initials of tech contact who has access to the machine    |
...

### Credentials
* Description of how credentials are being tracked at the centre (eg. password manager like Lastpass/Bitwarden, password databases like KeePass)
* The set process of how to obtain access to these manager clients (eg. who to talk to or where to find the 'Master' password) 

## Challenges
### Proxy issues
* What proxy was used at that centre
* URL whitelisting process
* Docker specific proxy setup
* Link to the this URL for all general proxy setup concerns (currently on GitHub [here](https://github.com/AustralianCancerDataNetwork/auscatverse/blob/main/guides/INFRASTRUCTURE.md#network-proxy), but point to the eventually deployed Sphinx page that will have that info)

### Troubleshooting
* Any particular issues unique to that centre which require attention from the centre's IT team
* Any potential hints, or "Gotchya's" that were picked up during the setup process (to potetnially benefit future setups)

## Deployment and Project work
### Docker and Docker Compose version
Which version of Docker and Docker Compose is running at the site

### Oncology Information System (OIS) available at the site
* Whether the site uses MOSAIQ, ARIA or EPIC and its version number
* If the site uses specific scripts to import data into their AusCAT node, and state where to find these scripts (eg. link to GitHub repository)

### Treatment Planning System (TPS) and/or PACS System available at the site for retrieving DICOM data
* Name of the system
* Does it require specialised scripting to get DICOM data out of these systems into the AusCAT? If so, who are the relevant contacts for this and where can these scripts be found?
 
### Pentaho
* Information about the Pentaho instance installed at the site (as this is not installed along with the AusCAT node and can vary between sites such as Pentaho version, Java version to run Pentaho, etc...)
* Filepath to the Pentaho application at the site
* Filepath to the Pentaho pipeline scripts that are used to populate the different AusCAT databases.
* Link to the this URL for general Pentaho setup (currently on GitHub [here](https://github.com/AustralianCancerDataNetwork/auscat_installation#pentaho-installation), but point to the eventually deployed Sphinx page that will have that info)

### Available AusCAT data
* Data that exists at that centre and ready in AusCAT's data stores

### Deployed stacks
* Could be description of proof of concept stack that was tested out at the centre
* Description of Production version of the AusCAT stack at the centre

### Clinical Projects
* Projects that the AusCAT stack at the centre was involved in 
