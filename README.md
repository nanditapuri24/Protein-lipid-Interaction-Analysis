# Protein-lipid-Interaction-Analysis
This repository details the workflow of systematic of Protein-Lipid of all PDB repository. 

<img width="385" height="385" alt="image" src="https://github.com/user-attachments/assets/31ea31c2-6f37-4c4e-976f-8b9d1b344644" />


****Step 1 : Remove Duplicate Binding site entries****

This step ensures that each protein–lipid complex contains only unique binding-site residue combinations.
If two rows have the same values in columns A, B, C, D, they represent the same binding site and one of them should be removed.

**Explanation**

The DataFrame is grouped by complex_PDB_ID so duplicates are removed within each complex, not globally. A row is considered valid only if at least one of the binding-site residue columns (A, B, C, D) is non-empty. Duplicate rows are detected when all four columns contain the same residue values. The first occurrence is kept; all subsequent duplicates are removed. Finally, all duplicate rows are dropped and the DataFrame index is reset.

**Why This Matters**

Protein–lipid interaction datasets often contain repeated rows caused by:
1. Multiple PLIP detections of the same interaction
2. Overlapping residues
3. Repeated parsing of the same complex

Removing duplicates ensures that:

Each binding-site residue pattern appears only once downstream analysis (counts, statistics, ML training) is not biased your dataset is clean, consistent, and non-redundant. 


****Step 2 : PLIP Analysis****

The second step to get the systematic analysis is to get the PLIP analysis. We have taken 











