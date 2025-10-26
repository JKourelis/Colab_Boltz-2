# Boltz-2 Google Colab Notebooks

<div align="center">
  <img src="https://raw.githubusercontent.com/jwohlwend/boltz/main/docs/boltz2_title.png" height="200" alt="Boltz-2 Logo">
</div>

<div align="center">

[![Single-Job](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1kL3eCnagNs7ZDPjLecCkRlk3w4xrHBFU?usp=sharing) [![CSV Batch](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1fl59SqNJo_W5LNiFFslAnzGnzWk4wn8E?usp=sharing)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT) [![Paper](https://img.shields.io/badge/bioRxiv-2025.06.14.659707-red.svg)](https://doi.org/10.1101/2025.06.14.659707)

</div>

Google Colab interfaces for Boltz-2 structure prediction. Predict protein, DNA, RNA, and ligand complexes.

---

## Which Notebook?

| | [Single-Job Notebook](https://colab.research.google.com/drive/1kL3eCnagNs7ZDPjLecCkRlk3w4xrHBFU?usp=sharing) | [CSV Batch Notebook](https://colab.research.google.com/drive/1fl59SqNJo_W5LNiFFslAnzGnzWk4wn8E?usp=sharing) |
|---|---|---|
| **Jobs per run** | 1 complex | Multiple complexes (batch) |
| **Input format** | Manual entry in cells | CSV file upload |
| **Sequences per complex** | Up to 10 | Up to 10 |
| **Homomultimers** | ✓ (copy numbers) | ✓ (copy numbers) |
| **PTMs/modifications** | ✓ (Cell 4.1) | ✓ (CSV column) |
| **Pocket restraints** | ✓ (Cell 4.2) | ✓ (CSV column) |
| **Covalent bonds** | ✓ (Cell 4.3) | ✓ (CSV column) |

**Use Single-Job for**: One complex at a time  
**Use CSV Batch for**: 5+ complexes in one run

---

## Single-Job Notebook

### Quick Start

1. Open [notebook](https://colab.research.google.com/drive/1kL3eCnagNs7ZDPjLecCkRlk3w4xrHBFU?usp=sharing)
2. Run Cell 1 (installation, ~3-5 min)
3. Fill Cell 2 (all sequences = ONE complex):
   - `jobname`: Any name (auto-generated if blank)
   - `seq1`: First sequence (protein/DNA/RNA/ligand)
   - `seq1_copies`: Number of copies (for homomultimers)
   - `seq2-seq10`: Additional sequences (leave blank if unused)
4. Run Cells 3-4 (MSA, advanced settings)
5. (Optional) Run Cells 4.1-4.3 (modifications/restraints)
6. Run prediction cell

### Examples

**Homodimer (2 identical proteins):**
```
seq1_name: A
seq1_type: protein  
seq1_content: MKTAYIAKQRQISFVKS
seq1_copies: 2
(Leave seq2-seq10 blank)
```

**Protein-ligand complex:**
```
seq1_name: A, seq1_type: protein, seq1_content: MKTAYIAK..., seq1_copies: 1
seq2_name: B, seq2_type: ligand, seq2_content: ATP, seq2_copies: 1
(Leave seq3-seq10 blank)
```

**Large assembly (A₂BC):**
```
seq1: Protein A, copies: 2
seq2: Protein B, copies: 1  
seq3: DNA C, copies: 1
(Result: 2xA + 1xB + 1xC in one complex)
```

---

## CSV Batch Notebook

### Quick Start

1. Open [notebook](https://colab.research.google.com/drive/1fl59SqNJo_W5LNiFFslAnzGnzWk4wn8E?usp=sharing)
2. Run Cell 1 (installation)
3. Download [empty template](boltz_csv_template.csv)
4. Fill CSV (see format below)
5. Run Cell 3 (upload CSV)
6. Run Cells 4-5 (MSA, advanced settings)
7. Run Cell 6 (batch prediction)

### CSV Format

**Template**: [boltz_csv_template.csv](boltz_csv_template.csv) (empty, all columns)

**Columns**: One row = one complex

- `jobname` - Unique identifier
- `seq1_name` to `seq10_name` - Chain IDs (A, B, C, etc.)
- `seq1_type` to `seq10_type` - Sequence type: `protein`, `dna`, `rna`, `ligand`
- `seq1` to `seq10` - Sequence content (amino acids, nucleotides, or ligand code)
- `seq1_copies` to `seq10_copies` - Number of copies (default: 1)
- `seq1_mods` to `seq10_mods` - Modifications (format: `CCD_CODE:POSITION;...`)
- `pocket_binder` - Binder chain ID (optional)
- `pocket_contacts` - Contact residues (format: `CHAIN:RES;CHAIN:RES;...`)
- `covalent_bonds` - Covalent bonds (format: `CHAIN:RES:ATOM:CHAIN:RES:ATOM;...`)

**Key points:**
- Leave unused seq columns empty (only fill seq1, seq2, etc. as needed)
- Use `copies` for homomultimers instead of repeating same sequence
- All sequences in one row form one complex

### CSV Examples

**1. Monomer:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies
MyProtein,A,protein,MKTAYIAKQRQISFVKS,1
```

**2. Homodimer (via copies):**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies
Homodimer,A,protein,MKTAYIAK,2
```

**3. Heterodimer:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq2_name,seq2_type,seq2,seq2_copies
Heterodimer,A,protein,MKTAYIAK,1,B,protein,MLKFSG,1
```

**4. Protein with phosphorylation:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq1_mods
WithPTM,A,protein,MKTAYIAK,1,CCD_SEP:8
```
*CCD_SEP = phosphoserine at position 8*

**5. Protein-ligand:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq2_name,seq2_type,seq2,seq2_copies
ProteinATP,A,protein,MKTAYIAK,1,B,ligand,ATP,1
```

**6. DNA-protein with DNA modification:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq1_mods,seq2_name,seq2_type,seq2,seq2_copies
DNAProtein,A,dna,ATCGATCG,1,CCD_5CM:3,B,protein,MLKFSG,1
```
*CCD_5CM = 5-methylcytosine at position 3*

**7. Complex assembly (A₂B₂C):**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq2_name,seq2_type,seq2,seq2_copies,seq3_name,seq3_type,seq3,seq3_copies
Assembly,A,protein,MKTAYIAK,2,B,protein,MLKFSG,2,C,protein,GSAKTI,1
```

**8. With pocket restraints:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq2_name,seq2_type,seq2,seq2_copies,pocket_binder,pocket_contacts
WithPocket,A,protein,MKTAYIAK,1,B,ligand,ATP,1,B,A:10;A:15;A:20
```
*Ligand B binds at protein A residues 10, 15, 20*

**9. With covalent bond:**
```csv
jobname,seq1_name,seq1_type,seq1,seq1_copies,seq2_name,seq2_type,seq2,seq2_copies,covalent_bonds
Covalent,A,protein,MKTAYIAK,1,B,ligand,HEM,1,A:6:CA:B:1:FE
```
*CA atom of A:6 bonded to FE atom of B:1*

### Embedded Reference Data

Built-in support for common modifications (upload custom reference if needed):

- **PTMs (15)**: SEP, TPO, PTR, MLY, ALY, HYP, M3L, MLZ, CSD, CSO, CGU, FME, NEP, HIC, CAS
- **Ligands (24)**: ATP, ADP, AMP, GTP, GDP, GMP, CTP, CDP, UTP, UDP, NAD, NAP, FAD, FMN, HEM, SAH, SAM, COA, ACO, PLP, TPP, BTN, MTA, THM
- **Ions (11)**: MG, ZN, CA, FE, FE2, MN, CU, NI, CO, K, NA, CL
- **Glycans (10)**: NAG, BMA, MAN, FUC, GAL, GLC, SIA, FUL, XYS, ARA
- **DNA mods (8)**: 5CM, 6OG, 8OG, 5HC, 5FC, 5CA, 6MA, 4MC
- **RNA mods (10)**: PSU, 5MC, M2G, M7G, 1MA, 5MU, OMC, OMG, 2MA, I

---

## Configuration Parameters

Both notebooks use same settings (Cells 3-4 in single-job, Cells 4-5 in CSV):

**Structure prediction:**
- `recycling_steps` (default: 6) - Refinement iterations. More = better quality + slower
- `sampling_steps` (default: 200) - Diffusion steps. More = smoother structures + slower  
- `diffusion_samples` (default: 5) - Number of models generated
- `max_parallel_samples` (default: 5) - GPU memory control
- `output_format` - `mmcif` or `pdb`

**MSA:**
- `msa_mode` - `mmseqs2_uniref_env` (default), `mmseqs2_uniref`, or `single_sequence`
- `msa_pairing_strategy` - `greedy` (default) or `complete`

**Affinity prediction:**
- `predict_affinity` (default: False) - Enable binding strength prediction (Kd/Ki)
- `sampling_steps_affinity` (default: 200) - Diffusion steps for affinity model
- `diffusion_samples_affinity` (default: 5) - Ensemble size for affinity

**Optional:**
- `write_full_pae` (default: False) - Predicted Aligned Error matrices
- `write_full_pde` (default: False) - Predicted Distance Error matrices  
- `use_potentials` (default: True) - Physics-based refinement

---

## Output Files

Each prediction generates:

- Structure files (mmCIF or PDB, 5 models default)
- Confidence scores (pLDDT per residue)
- PAE/PDE matrices (if enabled)
- Affinity predictions (if enabled)
- ZIP archive with all results
- Auto-upload to Google Drive (if configured)

---

## System Requirements

- **GPU**: T4/V100/A100 (A100 recommended for >1000 residues)
- **Runtime**: Must enable GPU in Colab settings
- **Internet**: Required for MSA server
- **Storage**: Google Drive recommended

---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| GPU memory error | Reduce `max_parallel_samples` or `diffusion_samples` |
| Installation fails | Runtime → Factory reset runtime |
| Slow MSA | Use `msa_mode: single_sequence` |
| CSV parsing error | Check column names match template exactly |
| "Unknown CCD code" | Use embedded reference data or upload custom file |
| Wrong NumPy version | Cell 1 handles this automatically (restarts runtime once) |

---

## Citations

**Boltz-2:**  
Passaro, S., Corso, G., Wohlwend, J., et al. (2025). Boltz-2: Towards Accurate and Efficient Binding Affinity Prediction. *bioRxiv*. [https://doi.org/10.1101/2025.06.14.659707](https://doi.org/10.1101/2025.06.14.659707)

**Boltz-1:**  
Wohlwend, J., Corso, G., Passaro, S., et al. (2024). Boltz-1: Democratizing Biomolecular Interaction Modeling. *bioRxiv*. [https://doi.org/10.1101/2024.11.19.624167](https://doi.org/10.1101/2024.11.19.624167)

**If using MSA:**  
Mirdita, M., Schütze, K., Moriwaki, Y., et al. (2022). ColabFold: making protein folding accessible to all. *Nature Methods*. [https://doi.org/10.1038/s41592-022-01488-1](https://doi.org/10.1038/s41592-022-01488-1)

<details>
<summary>BibTeX</summary>

```bibtex
@article{passaro2025boltz2,
    title={Boltz-2: Towards Accurate and Efficient Binding Affinity Prediction},
    author={Passaro, Silvia and Corso, Gabriele and Wohlwend, Jeremy and others},
    journal={bioRxiv},
    year={2025},
    doi={10.1101/2025.06.14.659707}
}

@article{wohlwend2024boltz1,
    title={Boltz-1: Democratizing Biomolecular Interaction Modeling},
    author={Wohlwend, Jeremy and Corso, Gabriele and Passaro, Silvia and others},
    journal={bioRxiv},
    year={2024},
    doi={10.1101/2024.11.19.624167}
}

@article{mirdita2022colabfold,
    title={ColabFold: making protein folding accessible to all},
    author={Mirdita, Milot and Sch{\"u}tze, Konstantin and Moriwaki, Yoshitaka and others},
    journal={Nature methods},
    year={2022},
    doi={10.1038/s41592-022-01488-1}
}
```

</details>

---

**Notebook Author**: Jiorgos Kourelis  
**Model**: Passaro et al., MIT CSAIL  
**License**: MIT (notebooks), see [official Boltz repo](https://github.com/jwohlwend/boltz) for model license