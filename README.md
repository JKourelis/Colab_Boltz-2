# Boltz-2: Unofficial Google Colab Interface

<div align="center">
  <img src="https://raw.githubusercontent.com/jwohlwend/boltz/main/docs/boltz2_title.png" height="200" alt="Boltz-2 Logo">
</div>

<div align="center">

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1kL3eCnagNs7ZDPjLecCkRlk3w4xrHBFU?usp=sharing)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Paper](https://img.shields.io/badge/bioRxiv-2025.06.14.659707-red.svg)](https://doi.org/10.1101/2025.06.14.659707)
[![Boltz-1](https://img.shields.io/badge/bioRxiv-2024.11.19.624167-red.svg)](https://doi.org/10.1101/2024.11.19.624167)

</div>

## Overview

This Google Colab notebook provides an easy-to-use interface for **Boltz-2**, a biomolecular foundation model developed by Passaro et al. (2025) that achieves AlphaFold3-level accuracy while running 1000x faster than traditional physics-based methods. This implementation is simply a user-friendly Colab wrapper that makes the original Boltz-2 model accessible through a step-by-step notebook interface.

**🚀 [Click here to open the Boltz-2 Colab notebook](https://colab.research.google.com/drive/1kL3eCnagNs7ZDPjLecCkRlk3w4xrHBFU?usp=sharing)**

> **Note**: This is an unofficial Google Colab implementation. All credit for the Boltz-2 model, methodology, and scientific contributions goes to the original authors. This notebook simply provides a convenient interface for running their published model.

## Key Features of This Interface

- **🧬 Access to Boltz-2**: Easy access to protein, DNA, RNA, and ligand complex prediction *(small molecules untested)*
- **⚡ Simplified Workflow**: Run the state-of-the-art Boltz-2 model through a user-friendly notebook
- **📊 Batch Processing**: Process multiple sequences or predict all combinations between two FASTA files
- **☁️ Google Drive Integration**: Automatic upload of results to your Google Drive
- **🔧 Advanced Configuration**: Comprehensive parameter control for specialized applications
- **📱 User-Friendly**: Clean step-by-step interface with progress tracking

## Input Methods

### 1. Manual Input
Enter sequences directly in the notebook with options for:
- Multiple protein, DNA, RNA sequences *(DNA/RNA well-tested)*
- Custom chain IDs and copy numbers
- Real-time sequence validation

### 2. FASTA Upload - Single File
Upload a FASTA file to process all sequences individually.

### 3. FASTA Upload - Combination Mode
Upload two FASTA files and predict **all combinations** between sequences in FASTA1 and FASTA2. Perfect for:
- Protein-protein interaction screening
- Large-scale structural comparisons
- Systematic binding analysis

## Configuration Parameters

### Structure Prediction Settings

- **`recycling_steps`** (default: 6): Iterative refinement passes. Each cycle improves local geometry and confidence scores. More steps = better quality but longer runtime.

- **`sampling_steps`** (default: 200): Diffusion denoising iterations. Controls structure generation quality. More steps = smoother structures but slower processing.

- **`diffusion_samples`** (default: 5): Number of independent structure predictions per input. More samples increase result reliability.

- **`max_parallel_samples`** (default: 5): GPU memory management. How many samples processed simultaneously. Critical for large complexes.

- **`step_scale`** (default: 1.638): Sampling temperature controlling generation randomness. Optimized default value.

### Binding Affinity Settings

- **`predict_affinity`** (default: False): Enable binding strength prediction (Kd/Ki values). Most reliable for protein-small molecule complexes.

- **`affinity_mw_correction`** (default: False): Apply molecular weight corrections to affinity predictions.

- **`sampling_steps_affinity`** (default: 200): Diffusion steps for affinity model.

- **`diffusion_samples_affinity`** (default: 5): Ensemble size for affinity predictions.

### Output Settings

- **`output_format`**: Choose between `mmcif` (recommended, more metadata) or `pdb` (wider compatibility).

- **`write_full_pae`** (default: False): Generate full Predicted Aligned Error matrices.

- **`write_full_pde`** (default: False): Generate full Predicted Distance Error matrices.

- **`use_potentials`** (default: True): Apply physics-based energy minimization for improved structure quality.

### MSA Configuration

- **`msa_mode`**: 
  - `mmseqs2_uniref_env`: Standard mode using ColabFold server
  - `mmseqs2_uniref`: Alternative MMseqs2 mode
  - `single_sequence`: Skip MSA generation
  - `custom`: Upload your own MSA file

- **`msa_pairing_strategy`**: 
  - `greedy`: Pair any taxonomically matching subsets
  - `complete`: All sequences must match

### Advanced Features

- **Covalent Restraints** *(untested)*: Define custom covalent bonds between specific atoms using format: `CHAIN:RESIDUE:ATOM:CHAIN:RESIDUE:ATOM`

- **Residue Modifications** *(untested)*: Specify amino acid modifications using format: `SEQ_ID:RESIDUE_INDEX:CCD_CODE`

- **Small Molecules** *(untested)*: Support for SMILES and CCD codes - functionality not yet validated

- **Google Drive Integration**: Automatic result upload to specified Google Drive folder with organized file structure.

## Workflow

1. **Installation**: Run the installation cell to set up Boltz-2 and dependencies
2. **Input Method**: Choose between Manual Input or FASTA Upload
3. **Configuration**: Configure sequences and prediction parameters
4. **MSA Settings**: Set up Multiple Sequence Alignment preferences
5. **Advanced Settings**: Adjust detailed prediction parameters
6. **Optional**: Configure covalent restraints or residue modifications
7. **Prediction**: Run Boltz-2 with progress monitoring
8. **Results**: Download results or access from Google Drive

## Output Files

Each prediction generates:
- **Structure files**: In mmCIF or PDB format
- **Confidence scores**: Per-residue confidence metrics
- **Affinity predictions**: When enabled, binding strength estimates
- **Results archive**: ZIP file containing all outputs
- **Google Drive upload**: Automatic cloud storage with organized folder structure

## System Requirements

- **Platform**: Google Colab with GPU runtime
- **Memory**: A100 GPU strongly recommended for reliable performance on larger complexes
- **Runtime**: T4, V100, or A100 GPUs supported (A100 preferred)
- **Internet**: Required for MSA server access and model downloads

## Citation

If you use this notebook in your research, please cite the Boltz papers:

**Boltz-2: Towards Accurate and Efficient Binding Affinity Prediction**  
Passaro, S., Corso, G., Wohlwend, J., Stärk, H., Pattanaik, L., Mendes, P., Velivckovic, P., Barzilay, R., & Jaakkola, T. (2025)  
*bioRxiv* [https://doi.org/10.1101/2025.06.14.659707](https://doi.org/10.1101/2025.06.14.659707)

**Boltz-1: Democratizing Biomolecular Interaction Modeling**  
Wohlwend, J., Corso, G., Passaro, S., Stärk, H., Pattanaik, L., Mendes, P., Velivckovic, P., Barzilay, R., & Jaakkola, T. (2024)  
*bioRxiv* [https://doi.org/10.1101/2024.11.19.624167](https://doi.org/10.1101/2024.11.19.624167)

In addition, if you use the automatic MSA generation, please cite:

**ColabFold: making protein folding accessible to all**  
Mirdita, M., Schütze, K., Moriwaki, Y., Heo, L., Ovchinnikov, S., & Steinegger, M. (2022)  
*Nature Methods* [https://doi.org/10.1038/s41592-022-01488-1](https://doi.org/10.1038/s41592-022-01488-1)

<details>
<summary>BibTeX format</summary>

```bibtex
@article{passaro2025boltz2,
    title={Boltz-2: Towards Accurate and Efficient Binding Affinity Prediction},
    author={Passaro, Silvia and Corso, Gabriele and Wohlwend, Jeremy and Stärk, Hannes and Pattanaik, Lagnajit and Mendes, Pedro and Velivckovic, Petar and Barzilay, Regina and Jaakkola, Tommi},
    journal={bioRxiv},
    year={2025},
    doi={10.1101/2025.06.14.659707}
}

@article{wohlwend2024boltz1,
    title={Boltz-1: Democratizing Biomolecular Interaction Modeling},
    author={Wohlwend, Jeremy and Corso, Gabriele and Passaro, Silvia and Stärk, Hannes and Pattanaik, Lagnajit and Mendes, Pedro and Velivckovic, Petar and Barzilay, Regina and Jaakkola, Tommi},
    journal={bioRxiv},
    year={2024},
    doi={10.1101/2024.11.19.624167}
}

@article{mirdita2022colabfold,
    title={ColabFold: making protein folding accessible to all},
    author={Mirdita, Milot and Sch{\"u}tze, Konstantin and Moriwaki, Yoshitaka and Heo, Lim and Ovchinnikov, Sergey and Steinegger, Martin},
    journal={Nature methods},
    year={2022},
    doi={10.1038/s41592-022-01488-1}
}
```

</details>

## Disclaimer

This Google Colab notebook is an independent implementation created to make the Boltz-2 model more accessible. All scientific credit, model development, and methodological contributions belong to the original Boltz team. This notebook serves purely as a user interface wrapper around their published work and does not represent any original research or model development.

For the official Boltz implementation and latest updates, please visit the [official Boltz repository](https://github.com/jwohlwend/boltz).

## Troubleshooting

**Common Issues:**
- **GPU Memory**: Reduce `max_parallel_samples` or `diffusion_samples` for large complexes
- **Runtime Errors**: Use "Factory reset runtime" in Google Colab
- **Slow MSA**: Switch to `single_sequence` mode if MSA server is slow
- **Upload Issues**: Check file formats and ensure proper FASTA formatting

## License

This Colab interface implementation is released under the MIT License. The Boltz-2 model itself is governed by its own license terms - please refer to the [official Boltz repository](https://github.com/jwohlwend/boltz) for model licensing information.

## Related Resources

- [Official Boltz Repository](https://github.com/jwohlwend/boltz)
- [Boltz-2 Paper](https://doi.org/10.1101/2025.06.14.659707)
- [ColabFold MSA Server](https://colabfold.com/)

---

**Author**: Jiorgos Kourelis  
**Last Updated**: July 24, 2025
