# model_train_infer - Template Repository for MLOps Projects on Databricks

## 🔎 Overview
`model_train_infer` is a reusable, modular template repository designed for automated machine learning workflows on Databricks. This repo contains production-ready notebooks for training and inference and is automatically cloned into new project repositories created by the `cli_gh` automation tool.

It acts as the **base repository** from which GitHub Actions pipelines clone assets like notebooks and config files, enabling scalable, consistent onboarding of MLOps jobs across multiple use cases and environments.

---

## ➕ How This Repo is Used
This template repo is **not run directly**. It is leveraged by:

### 1. [cli_gh](https://github.com/Ashoke238/cli_gh)
- Automates new repo creation by cloning this template.
- Creates a `main` branch, pushes template contents.
- Creates a `dev` branch for development/testing workflows.
- Updates `mlops_config/mlops_config_dev.json` with job metadata.

### 2. [std_ml_ops_pipeline_components](https://github.com/Ashoke238/std_ml_ops_pipeline_components)
- Used during job execution.
- Imports modules dynamically to run ML workflows via Databricks Jobs.

> **Important**: This repo should not be modified directly unless you're updating the global notebook or config template.

---

## 🔹 Folder Structure
```
model_train_infer/
├── mlops_config/
│   └── mlops_config_dev.json   # Placeholder for dynamic job config
└── notebooks/
    ├── Demo_train_Notebook1.py # Training job notebook
    └── Demo_inference_Notebook1.py # Inference job notebook
```

---

## 📊 Notebooks Explained

### 🌟 `Demo_train_Notebook1.py`
- Loads a classification dataset.
- Performs feature engineering.
- Trains a simple classification model (e.g., logistic regression or decision tree).
- Logs metrics & parameters via MLflow.
- Registers the model to MLflow model registry.
- Persists any relevant artifacts.

### ⚖️ `Demo_inference_Notebook1.py`
- Loads a previously registered model from MLflow.
- Loads test data.
- Performs batch inference.
- Saves predictions to Delta table or Databricks workspace directory.
- Logs inference metadata (optional).

---

## 🌐 GitHub Actions (Inherited)
When cloned via `cli_gh`, the new repo will automatically:
- Inherit a GitHub Actions workflow (from the CLI repo) for:
  - Running the training job every 30 days.
  - Running the inference job every hour.
  - Triggering the CLI via `workflow_dispatch`.

---

## 🚀 Example Flow
1. You invoke CLI from `cli_gh`:
   ```bash
   python cli/main.py --repo-name my_new_project --accuracy-train 0.85 --accuracy-inference 0.80
   ```
2. The CLI:
   - Creates a GitHub repo `my_new_project`
   - Clones this template repo
   - Creates `main` and `dev` branches
   - Imports the new repo into Databricks
   - Creates train/infer jobs pointing to notebooks in this repo
   - Updates `mlops_config_dev.json` with job metadata
3. A GitHub Actions workflow inside the new repo runs periodically
   - Triggers `Demo_train_Notebook1.py` every 30 days
   - Triggers `Demo_inference_Notebook1.py` every hour

---

## 🌐 Config File - `mlops_config_dev.json`
This file is updated dynamically post-job creation. Sample structure:
```json
{
  "train_job_id": "123456789",
  "infer_job_id": "987654321",
  "accuracy_train_threshold": 0.85,
  "accuracy_inference_threshold": 0.80
}
```

---

## ⚡ Best Practices
- Do not commit local changes to this repo unless you're modifying the **template**.
- Maintain versioned, clean notebooks.
- Add comments and structure your notebooks for CI/CD execution.
- Avoid using UI widgets or notebook-only features.

---

## ✉️ Contributing
If you'd like to enhance this template (e.g., by adding a new notebook structure or logging improvements), fork and raise a PR.

---

## 🌟 Credits
This repository is a foundational asset in the automated MLOps workflow developed by [Ashoke238](https://github.com/Ashoke238).

> Template. Reusable. Reliable. 🚀

