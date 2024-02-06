# Predicting small-molecule inhibition of protein complexes

## Abstract

**Motivation:**
<br>Protein-Protein Interactions (PPIs) have key roles in biological processes and are involved in numerous diseases, making discovery of PPI inhibitors a key focus in drug development. Traditional approaches for this purpose involve expensive and time-consuming experiments in the wet lab. Such drug development efforts can greatly benefit from novel machine learning methods to predict inhibitors of specific PPIs. To the best of our knowledge, there is no existing method that takes a protein complex and a compound as inputs to predict whether the given compound acts as an inhibitor of that complex.</br>
**Methods:**
<br>We present a graph neural network approach that takes the structure of a protein complex and the SMILES representation of a compound to predict the potential of the compound to inhibit the interaction between proteins in the given complex in a targeted manner.</br>
**Results:**
<br>The proposed method, trained and validated on 714 inhibitors of 23 complexes in the 2p2i-DB-v2 database, demonstrates superior predictive accuracy (cross-validation AUC-ROC of 0.85) outperforming baseline kernel methods and pre-trained neural network approaches. We further tested the predictive performance of our model on two independent external datasets – one collected from recent publications and another consisting of putative inhibitors of the SARS-CoV-2-Spike and Human-ACE2 protein complex with AUC-ROCs of 0.82 and 0.78, respectively. This pioneering application represents the first successful method for predicting specific protein complex inhibitors for drug design and development and lays the groundwork for future model development in this vital field.</br>

## Set Up Environment
```
python=3.6
!pip install rdkit
pip install biopython

```
## Dataset
2p2iComplexPairs Dataset can be downloaded from this link [https://github.com/adibayaseen/PPI-Inhibitors/blob/01ad4975fb9133825b1bf9e71b64fcdaaa5e4d8b/Data/2p2iComplexPairs.txt]<br/>
2p2iInhibitorsSMILES.txt Dataset can be downloaded from this link [https://github.com/adibayaseen/PPI-Inhibitors/blob/01ad4975fb9133825b1bf9e71b64fcdaaa5e4d8b/Data/2p2iInhibitorsSMILES.txt]<br/>
Binders With Tanimoto Similarity 0.85 dataset from here [https://github.com/adibayaseen/PPI-Inhibitors/blob/01ad4975fb9133825b1bf9e71b64fcdaaa5e4d8b/Data/Binders%20With%20Tanimoto%20Similarity%200.85.csv] <br/>
SuperDrug2 Compounds are used for Random Negative examples can downloaded from here  <br> [https://github.com/adibayaseen/PPI-Inhibitors/blob/b1e45884f61f792399abad2e4492f48083ab1093/Data/approved_drugs_chemical_structure_identifiers.xlsx]<br/>

# Data discription
# Input File used in All Methods 
WriteAllexamplesRandomBindersIdsAll_24JAN_Binary.txt can be downloaded from this link [https://github.com/adibayaseen/PPI-Inhibitors/blob/2d6bd03422602ec19147870c487e64018b52660f/Data/WriteAllexamplesRandomBindersIdsAll_24JAN_Binary.txt]<br/>
```
> Name of Complex Example Belongs<space> Target Complex name <space> Inhibitor Name  <space> Label <newline> <br/>
* E.g 3UVW_A_2_B 3UVW_A_2_B WSH 1.0
```
## 2p2iComplexPairs.txt
* Input File format <br/>
```
> Name of Complex pair<space> Target chain <space>Target chain Sequence <space>Off Target chain  <space> Off Target chain Sequence<newline> <br/>
1YCR_A_2_B<space>A<space>ETLVRPKPLLLKLLKSVGAQKDTYTMKEVLFYLGQYIMTKRLYDEKQQHIVYCSNDLLGDLFGVPSFSVKEHRKIYTMIYRNLVVvB<space>ETFSDLWKLLPEN <newline><br/>

```
## 2p2iInhibitorsSMILES.txt
* Input File format <br/>
```
> Name of Complex pair<space> Name of the Inhibitor<space>Complex name <space>Inhibted Complex <space> SMILES of the Inhibitor<space>Label<newline> <br/>
1YCR_A_2_B<space>1RV1<space>1YCR<space>IMZ<space>CCOc1cc(ccc1C2=N[C@H]([C@H](N2C(=O)N3CCN(CC3)CCO)c4ccc(cc4)Br)c5ccc(cc5)Br)OC <newline><br/>
1YCR_A_2_B 1RV1 1YCR IMZ CCOc1cc(ccc1C2=N[C@H]([C@H](N2C(=O)N3CCN(CC3)CCO)c4ccc(cc4)Br)c5ccc(cc5)Br)OC  1
1YCR_A_2_B 1T4E 1YCR DIZ c1cc(ccc1[C@H]2C(=O)Nc3ccc(cc3C(=O)N2[C@@H](c4ccc(cc4)Cl)C(=O)O)I)Cl  1
1YCR_A_2_B 2LZG 1YCR 13Q c1cc(cc(c1)Cl)[C@H]2C[C@@H](C(=O)N([C@@H]2c3ccc(cc3)Cl)CC4CC4)CC(=O)O  1
```
## Binders With Tanimoto Similarity 0.85
* Input File format <br/>
```
>(Complex,inhibitor pair)<Next Column> inhibitor Names<Next Column>Binders SMILES<Next Column>Similarity<newline><br/>
1YCR_A_2_B<Next Column> "[array(['IMZ', 'DIZ', '13Q', 'YIN', 'K23', 'MI6', 'TJ2', '07G', 'VZV',
       'LTZ', 'BLF', '0R2', '0R3', '0Y7', 'NUT', '1MN', '1MO', '1MQ',
       '1MT', '1MY', 'Y30', '28W', '2SW', '2TW', '2TZ', '2U0', '2U1',
       '2U5', '2U6', '2U7', '2V8', '35S', '35T', '3UD', '4SS', '4T4',
       '4TH', '62R', '62T', '62Q', 'NUT', '6ZT', '4NJ', '4NX', '6SK',
       '6SJ', '6SS', '6ST', '7HC', '6GG', '6GG', '9QW', 'NUT', 'EYH'],
      dtype='<U3')]"	<Next Column> OC(=O)c1ccc2c3C[C@H]4[C@H]([C@H](c5cccc(Cl)c5F)[C@@]5(N4CC4CC4)C(=O)Nc4nc(Cl)ccc54)n3nc2c1<Next Column> 0.84
 <newline><br/>
```
## Detail of Binders in WriteAllexamplesRandomBindersIdsAll_24JAN_Binary.txt
```
Complexname,Binders SMILES
1YCR_A_2_B,OC(=O)c1ccc2c3C[C@H]4[C@H]([C@H](c5cccc(Cl)c5F)[C@@]5(N4CC4CC4)C(=O)Nc4nc(Cl)ccc54)n3nc2c1
```
Unique number for set of binders are saved in WriteAllexamplesRandomBindersIdsAll_24JAN_Binary.txt
Detail of Binders in WriteAllexamplesRandomBindersIdsAll_24JAN_Binary.txt can be downloaded here <br>[https://github.com/adibayaseen/PPI-Inhibitors/blob/b1e45884f61f792399abad2e4492f48083ab1093/Data/BindersWithComplexname.csv]<br/>

GNN-based pipeline Protein complex Features for Positive data (2p2i complexes) Drive link is here <br> [https://drive.google.com/file/d/1goeDiPZSKT1Xx3j00eNG9xlqYkLLv1gW/view?usp=sharing]<br/>
GNN-based pipeline DBD5 Protein complex Features for Negative examples, Drive link is here  <br> [https://drive.google.com/file/d/1GOYEKLQCoGea9QQ72kujy0rdJKbUSYAE/view?usp=sharing]<br/>
# Code
## Setting the path of Input or Clone Github repository 
 <br> Clone GitHub repository to collab and give githubpath as /content/PPI-Inhibitors </br>
 ```
 !git clone https://github.com/adibayaseen/PPI-Inhibitors
```
 <br> Download 2p2i complex features and DBD5 complex features from Google Drive and path with folder name GNN-PPI-Inhibitor  </br>
## GNN-based Results
GNN-based pipeline Result can be reconstructed using this collab code <br> [https://github.com/adibayaseen/PPI-Inhibitors/blob/9d04108e683601a393ecd0e733ac6e65207eb8a3/code/GNN_based_pipeline_Training_for_Predicting_small_molecule_inhibition_of_protein_complexes_ipynb.ipynb] </br>
## SVM Results
Baseline method (SVM) Result can be reconstructed using this collab code <br> [https://github.com/adibayaseen/PPI-Inhibitors/blob/9d04108e683601a393ecd0e733ac6e65207eb8a3/code/svmreadfromfile_generate_prediction_binders_and_random_both_as_negative.ipynb] </br>
##  GearNet Embedding Results
GearNet Embedding results can be reconstructed using the link <br> [https://github.com/adibayaseen/PPI-Inhibitors/blob/9d04108e683601a393ecd0e733ac6e65207eb8a3/code/GearNet%20Embedding.ipynb]</br>
# Results 
Our GNN-based pipeline results are better than our Baseline method (SVM) and GearNet Embedding. All result can be reconstructed using colab notebooks linked in code section<br> 
![AUC-ROC Comparative Results ](https://github.com/adibayaseen/PPI-Inhibitors/blob/e49a2d6b091c3c174c5cab9a30d732374286935a/Final%20Results/Figures/Final%20Aucroc.png)
AUc-PR results are shown below
![AUC-PR Comparative Results ](https://github.com/adibayaseen/PPI-Inhibitors/blob/e49a2d6b091c3c174c5cab9a30d732374286935a/Final%20Results/Figures/Final%20Aucpr.png)
