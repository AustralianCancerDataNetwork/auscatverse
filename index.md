# AusCAT

![ARDC](/overview/images/ARDC.png)

The Australian Computer Assisted Theragnotics (AusCAT) is a federated learning software platform that enables linking of radiation oncology treatment data and outcomes data across hospitals and with state and national registries to allow development of unique models from the combined datasets. The goal of AusCAT is to improve accessibility and governance structures to support data users including clinicians, data scientists, governments and policy makers. AusCAT streamlines data analysis, administrative, ethical, and political aspects of research, enabling learning across datasets that are otherwise difficult to merge due to size or ethical and governance challenges.

![AusCAT architecture 1](/overview/images/Auscat_1.png)

## Why is AusCAT necessary?

Direct applicability of clinical trial evidence for all patients even when technology hasn’t moved on can also be challenging. As a community we are currently only enrolling about 3% of our patients in clinical trials and due to the required tight eligibility of clinical trials it’s arguable over if this evidence is directly applicable. Its estimated over 60% of patients don’t meet the eligibility criteria- that’s a lot of patients where clinical trial evidence doesn’t provide direct guidance on treatment decisions.

![AusCAT architecture 2](/overview/images/Auscat_2.png)

## AusCAT data and tools for institutions and researchers

Having an AusCAT node set-up at your institution provides the opportunity to use your data as part of a broader network to answer key questions, those related to clinical questions that need breadth and depth of data and questions related to using federated learning with real world data. This includes collaborations with other clinical centres and clinical and machine learning research groups. Involvement in individual projects run on the AusCAT node will be a decision of the individual centres/nodes, it’s likely that not all centres will be involved in all projects depending on interest, data availability and resources at the time.

The AusCAT platform also provides data visualisation tools to allow research and institution to easily identify what data is available at their institutions using the Data availability Dashboard. Database online viewer PGadmin allows users to inspect what is available in the database. The Orthanc server provides a front-end to browse DICOM patient data (dose files, RT structures, treatment imaging (CT, PET, MRI, CBCT)).

```{toctree}
overview/index.md
:hidden:
```

```{toctree}
:caption: Getting Started
:hidden:

installation
examples
```

```{toctree}
:caption: Components
:hidden:

components/imaging
```

```{toctree}
:caption: For Developers
:hidden:

guidelines/CODING

```
