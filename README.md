# 🧠 model_train_infer

This repository contains two core Databricks notebooks designed for a simple machine learning pipeline: model training and inference. These notebooks are invoked as part of a fully automated CI/CD pipeline driven by [cli_gh](https://github.com/Ashoke238/cli_gh), and integrated with MLflow and Unity Catalog for model management.

---

## 📂 Repository Structure

```bash
model_train_infer/
├── mlops_config/
│   └── mlops_config_dev.json       # Contains training/inference job IDs populated by CLI automation
├── notebooks/
│   ├── Demo_train_Notebook1.py     # Training notebook
│   └── Demo_inference_Notebook1.py # Inference notebook
├── .github/
│   └── workflows/
│       └── ci.yml                  # GitHub Actions workflow to execute training/inference
├── requirements.txt               # Python package dependencies
└── README.md                      # This file
```

---

## 📘 Training Notebook - `Demo_train_Notebook1.py`

This notebook performs end-to-end training of a classification model using a real dataset. Below are the steps:

### ✅ Data Ingestion
- Loads the Breast Cancer dataset using `sklearn.datasets.load_breast_cancer()`.
- Converts it to a Pandas DataFrame and assigns feature and label columns.

### 🧪 Feature Engineering
- Scales all features using `StandardScaler`.
- Splits the dataset into train and test using an 80-20 split.

### 🧠 Model Training
- Uses `LogisticRegression` from `sklearn.linear_model` with default parameters.
- Trains on scaled features.

### 📊 MLflow Integration
- Logs hyperparameters (e.g., model type, scaler used, split ratio).
- Logs performance metric `accuracy`.
- Registers the model to **MLflow Model Registry**.
- Naming convention for experiment and model includes GitHub repo name, environment (dev), and user email.
- Model is tracked in Unity Catalog-backed MLflow registry for cross-workspace access.

### 💾 Dataset Storage
- Persists the training dataset (`X_train`, `y_train`) to Unity Catalog as Delta table.
- Table name format: `uc_catalog.uc_schema.{repo_name}_train_dataset_dev`

---

## ⚖️ Inference Notebook - `Demo_inference_Notebook1.py`

This notebook performs inference using the latest registered model.

### 🔄 Load Model
- Retrieves the latest model version from MLflow for the given repo and `dev` environment.

### 📥 Load Test Data
- Reads test dataset from the Delta table saved during training phase.

### 🔮 Perform Inference
- Loads the model and runs predictions on the test dataset.

### 📁 Save Results
- Saves prediction results as a Delta table to Unity Catalog.
- Table name format: `uc_catalog.uc_schema.{repo_name}_predictions_dev`

### 📌 MLflow Metadata Logging
- Logs metadata about the inference run (model version, run ID, timestamp).

---

## 🤖 Workflow Automation

The CLI automation in [cli_gh](https://github.com/Ashoke238/cli_gh) performs the following steps:

1. Accepts inputs like repo name, training & inference accuracy thresholds.
2. Creates a new GitHub repo from a template.
3. Pushes all notebook contents and JSON config.
4. Imports the repo into Databricks.
5. Creates scheduled jobs in Databricks for training and inference.
6. Populates the `mlops_config_dev.json` with the generated job IDs.

---

## ✅ Sample Configuration File

```json
{
  "train_job_id": 123456789,
  "infer_job_id": 987654321
}
```

This file is updated by the CLI after job creation.

---

## 🛠️ Requirements

- Databricks Runtime: DBR 13.x or above
- Unity Catalog-enabled workspace
- GitHub repository secrets configured for token and workspace info

---

## 🙌 Contributing

PRs are welcome! If you’d like to suggest improvements or enhancements to the notebooks or pipeline, feel free to open an issue.

---

## 📄 License

MIT © Ashoke238

