# 3D Structural Analysis of Plasmodium Falciparum to Detect Inhibitors
#### Syed Arham Wasti - arhamwasti@gmail.com

Machine learning models hold the capability to make innovative discoveries in healthcare and medicinal fields. They possess abilities that transcend human limitations, enabling them to perform complicated pattern analyses rapidly and craft solutions to problems beyond human capability. In this paper, I explore ways that machine learning can be applied to make advances in drug discovery. Particularly targeting treatment for malaria, a deadly disease primarily caused by the parasite Plasmodium falciparum, imposes a substantial global health impact. The nature of traditional drug discovery methods is time-consuming and expensive, ultimately resulting in limited treatment development. This project investigates using advanced deep learning models to expedite and enhance the drug development process.

## Method

My proposed framework which proved to stand effective is composed of two distinct components, an ESMFold model that uses an input amino acid sequence to predict a 3D protein structure and generate a format representing the sequence, as well as a custom fine-tuned graph convolutional network (GCN) that leverages convolutional molecular features to spatially and contextually understand my custom P. falciparum dataset.

![Pipeline Flow Diagram](https://raw.githubusercontent.com/ArhamWasti/Plasmodium-Falciparum-Inhibitor-Detection/main/Pipeline%20Flow%20Diagram.svg?token=GHSAT0AAAAAACSZFUEU5WHVW6R2WD36BXRKZT6QDEQ)

#### AminoAcid to PDB: 
This component relies upon the revolutionary capabilities of Meta’s ESMFold model. This model allows us to input a string containing a sequence of amino acids in single-letter code, and receive a predicted three-dimensional structure of the protein. This prediction is outputted in a PDB format, which can be used to visualize the structure with precision in molecular visualization tools. 

#### Molecular Graph Convolutional Network 
Once a PDB is received from ESMFold, it is parsed to create an RDKit molecule object. This object is necessary for mapping each molecule into graph nodes that correspond to atoms and edges that represent bonds. The feature object representing our given PDB is provided to my model for inference, outputting logits indicating probabilities for this binary classification task. With the use of an argmax, we can determine which label the model deemed most likely, and thus determine if inhibitors were predicted to be present in the PDB structure. The GCN model, pre-trained on molecular structures, was fine-tuned in a similar manner. As previously mentioned, I curated a dataset of PDBs scraped through various PDB banks, collecting as many P. falciparum PDB representations as reasonably available. These were labeled using an inhibitor check with a Biopython loop and an imbalance in the data was handled accordingly. Preprocessing was carried out by , with the PDBs being featurized and stored in a dataset. This curated dataset was employed during the training phase, where the model was fit to our data on a binary classification upstream task, with hyperparameters suitable for the low-resource nature of the task. Batch size and learning rates were adjusted to strike a balance between labels exposed to the model during each epoch.

## Results
With an array of choices concerning molecular models and featurizers, selecting the correct model is a make-or-break decision. Area Under Curve (AUC) scores were used to evaluate the model throughout the training process, with an 80-10-10 split due to the limited dataset size. The initial use of ProgressiveMultiTask with BindingPocketFeaturizer struggled to achieve generalization accuracy and did not show valuable learning potential. Switching to GCN pre-trained on molecular structures, I was able to leverage computed graph representations of PDBs with a model that could spatially comprehend the patterns and features indicative of inhibitor presence.

![Protein Structure Labels](https://raw.githubusercontent.com/ArhamWasti/Plasmodium-Falciparum-Inhibitor-Detection/main/Protein%20Structure%20Labels.svg?token=GHSAT0AAAAAACSZFUEVLF5RYUJZSPIM5MXEZT6QCGQ)

## Run Instructions

To install all necessary packages, run this command in your environment's terminal

```
pip install -r requirements.txt
```

## Dataset
https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/A21A7R 
