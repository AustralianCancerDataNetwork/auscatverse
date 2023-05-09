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
| Machine host name             | IP Address    | Type                      | Specs                         | Contact Access                                            |
| ---                           | ---           | ---                       | ---                           | ---                                                       |
| name1.domain.nswhealth.net    | 10.x.x.x      | Ubuntu VM                 | CPU, Memory, disk space, GPU  | Initials of tech contact who has access to the machine    |
| name2.domain.nswhealth.net    | 10.x.x.x      | Cloud VM Ubuntu           | CPU, Memory, disk space, GPU  | Initials of tech contact who has access to the machine    |
...


## Challenges
### Proxy issues
* What proxy was used at that centre
* URL whitelisting process
* Docker specific proxy setup

### Troubleshooting
* Any particular issues unique to that centre which require attention from the centre's IT team
* Any potential hints, or "Gotchya's" that were picked up during the setup process (to potetnially benefit future setups)

## Deployment and Project work
### Available AusCAT data
* Data that exists at that centre and ready in AusCAT's data stores
### Deployed stacks
* Could be description of proof of concept stack that was tested out at the centre
* Description of Production version of the AusCAT stack at the centre
### Clinical Projects
* Projects that the AusCAT stack at the centre was involved in 
