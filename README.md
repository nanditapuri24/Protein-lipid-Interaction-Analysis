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

______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

****Step 2 : PLIP Analysis****

This step performs large-scale protein–lipid interaction profiling using PLIP (Protein–Ligand Interaction Profiler) on an HPC cluster. We use a SLURM batch script that loops through thousands of PDB structures, runs PLIP for each file, and stores all interaction fingerprints in structured output folders.

🔧 Overview

PLIP automatically detects and characterizes:
1. Hydrogen bonds
2. Hydrophobic contacts
3. π–stacking and π–cation interactions
4. Salt bridges
5. Halogen bonds
6. Water-mediated interactions
7. Atom-level donor/acceptor roles
8. Interaction geometry and distances

These features form the basis for downstream systematic lipid–protein interaction analysis, ML modeling, and pocket chemistry profiling.
______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________


****Step 3 : PLIP Analysis: Summarizing PLIP Interaction Counts (Bash Parser)****

After generating PLIP interaction reports for each PDB file, this script parses the report.txt outputs and counts the number of interactions of each type across the entire lipid class. This script uses awk to extract and count interaction rows from each section of the PLIP report and produces a single summary file containing the totals.

📌 Purpose of This Script

PLIP produces detailed interaction profiles in human-readable .txt files. Each file has sections such as:

1. Hydrophobic Interactions
2. Hydrogen Bonds
3. Water Bridges
4. π–Stacking interactions
5. π–Cation interactions
6. Halogen Bonds
7. Salt Bridges
8. Metal Complexation

This script:
1. Iterates through every PLIP result folder
2. Reads the corresponding report.txt
3. Counts how many interactions appear in each section
4. Aggregates totals across the entire lipid class (e.g., Glycerophospholipid)
5. Writes final totals to an output summary file

______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

⭐ ****Step 4: PFAM Family Analysis Using PLIP****

This Jupyter notebook performs a PFAM family–level analysis of the lipid–protein interactions identified in previous steps. The goal is to understand which PFAM domains are most commonly associated with each lipid class and how their interaction patterns differ.

🔍 What This Step Does

1. Identify Unique PFAM Families: From the annotated dataset, we extract all PFAM domains associated with the proteins that bind each lipid class.

2. Determine the Most Frequent PFAM Families: We compute the top recurring PFAM families to understand which structural domains are enriched for lipid binding.

3. Link PFAM Families to PLIP Interaction Profiles: For each PFAM family, we locate the corresponding PLIP report.txt files and extract their interaction features.

______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________

🧬 ****Step 5: Amino Acid Frequency Analysis from PLIP Reports****

This script processes all PLIP report.txt files for a given lipid class and extracts the amino acids involved in each interaction type. It then computes and summarizes how frequently each amino acid appears across all interactions. This allows you to analyze which residues dominate hydrophobic contacts, hydrogen bonds, salt bridges, π–interactions, etc., for each lipid class.

🔍 What This Script Does

1. Traverses all PLIP report folders for a specific lipid class
2. Parses each interaction section (Hydrophobic, Hydrogen Bonds, Water Bridges, Salt Bridges, etc.)
3. Extracts the amino acid (3-letter codes) from each table line
4. Saves amino acids per interaction type into temporary files
5. Aggregates and sorts the amino acid frequencies
6. Writes the final counts into a single .txt summary file

______________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________










