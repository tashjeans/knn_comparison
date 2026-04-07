# Baseline Inputs for TCR–pMHC Classification

This folder provides the minimum required inputs to compare:

- Plain ESM embeddings
- Fine-tuned ESM embeddings

The goal is to build a simple downstream classifier (e.g. KNN).

---

## Contents

data/
    train_df_clean.csv
    val_df_clean_pos_neg.csv
    test_df_clean_pos_neg.csv

embeddings/no_boltz/
    train/shard_*.pt
    val/shard_*.pt
    test/shard_*.pt

scripts/
    prepare_baseline_inputs.py

---

## What the script does

The script:

1. Loads raw sequence data from CSV
2. Generates embeddings using plain ESM
3. Loads precomputed fine-tuned embeddings (shards)
4. Converts both into fixed-size vectors:
   [TCR | peptide | HLA]
5. Saves outputs as .npz files

---

## Output format

Each .npz file contains:

- X: feature matrix (N x D)
- y: labels (0/1)
- pair_ids: identifiers

---

## What you need to do

You should:

- Load the .npz files
- Build your own classifier (e.g. KNN)
- Compare performance between:
  - plain ESM
  - fine-tuned ESM

---

## Environment

Required packages:

- torch
- transformers
- numpy
- pandas

---

## Notes

- "Fine-tuned ESM" here refers to embeddings already generated from a trained model.
You do NOT need to retrain anything.

- This repository provides fixed-size representations of TCR–pMHC pairs for downstream classification. These representations are derived from token-level embeddings using mean pooling. The original modelling pipeline uses learned projection heads and a Hamiltonian-based objective to map variable-length sequences into structured latent representations.

- Mean pooling provides a deterministic way to convert variable-length token embeddings into a single vector, however if you want to do something more interesting with the embeddings, you are more than welcome to.

- Embedding shards are not included in this repository due to size. I will send them separately for you to place under embeddings/no_boltz/

