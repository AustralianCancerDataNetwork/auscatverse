
# AusCAT

The Australian Computer Assisted Theragnotics (AusCAT) is a federated learning software platform that enables linking of radiation oncology treatment data and outcomes data across hospitals and with state and national registries to allow development of unique models from the combined datasets. Goal of AusCAT is to improve accessibility and governance structures to support data users including clinicians, data scientists, governments and policy makers. AusCAT streamlines data analysis, administrative, ethical, and political aspects of research, enabling learning across datasets that are otherwise difficult to merge due to size or ethical and governance challenges.


![AusCAT architecture 1](/images/Auscat_1.png)

## Why is AusCAT necessary?

Direct applicability of clinical trial evidence for all patients even when technology hasn’t moved on can also be challenging. As a community we are currently only enrolling about 3% of our patients in clinical trials and due to the required tight eligibility of clinical trials it’s arguable over if this evidence is directly applicable. Its estimated over 60% of patients don’t meet the eligibility criteria- that’s a lot of patients where clinical trial evidence doesn’t provide direct guidance on treatment decisions.

![AusCAT architecture 2](/images/Auscat_2.png)

### AusCAT data and tools for institutions and researchers

Having an AusCAT node set-up at your institution provides the opportunity to use your data as part of a broader network to answer key questions, those related to clinical questions that need breadth and depth of data and questions related to using federated learning with real world data. This includes collaborations with other clinical centres and clinical and machine learning research groups. Involvement in individual projects run on the AusCAT node will be a decision of the individual centres/nodes, it’s likely that not all centres will be involved in all projects depending on interest, data availability and resources at the time.

AusCAT platform also provides data visualisation tool to allow research and institution to easily identify what data is available at their institutions using the Data availability Dashboard tool. Database online viewer PGadmin allows users to inspect what is available on the database. Orthanc server to browse DICOM patient data (dose files, RT structures, treatment imaging (CT, PET, MRI, CBCT)).


### Publications:

Finnegan RN, Chin V, Chlap P, Haidar A, Otton J, Dowling J, Thwaites DI, Vinod SK, Delaney GP, Holloway L. Open-source, fully-automated hybrid cardiac substructure segmentation: development and optimisation. Physical and Engineering Sciences in Medicine. 2023 Feb 13:1-7.

Haidar A, Field M, Batumalai V, Cloak K, Al Mouiee D, Chlap P, Huang X, Chin V, Aly F, Carolan M, Sykes J. Standardising Breast Radiotherapy Structure Naming Conventions: A Machine Learning Approach. Cancers. 2023 Jan;15(3):564.

Field M, Thwaites DI, Carolan M, Delaney GP, Lehmann J, Sykes J, Vinod S, Holloway L. Infrastructure platform for privacy-preserving distributed machine learning development of computer-assisted theragnostics in cancer. Journal of Biomedical Informatics. 2022 Oct 1;134:104181.

Hansen CR, Price G, Field M, Sarup N, Zukauskaite R, Johansen J, Eriksen JG, Aly F, McPartlin A, Holloway L, Thwaites D. Open-source distributed learning validation for a larynx cancer survival model following radiotherapy. Radiotherapy and Oncology. 2022 Aug 1;173:319-26.

Field M, Vinod S, Aherne N, Carolan M, Dekker A, Delaney G, Greenham S, Hau E, Lehmann J, Ludbrook J, Miller A. Implementation of the Australian Computer‐Assisted Theragnostics (AusCAT) network for radiation oncology data extraction, reporting and distributed learning. Journal of Medical Imaging and Radiation Oncology. 2021 Aug;65(5):627-36.

```{toctree}
:caption: Getting Started
:hidden:

installation
examples
```

```{toctree}
:caption: For Developers
:hidden:

guidelines/CODING

```
