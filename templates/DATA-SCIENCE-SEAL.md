# SEAL Template: Data Science & ML Projects

Plug-and-play SEAL configuration for data science, machine learning, and AI projects.

## Quick Start

```bash
# Initialize SEAL for data science
/seal-start --template data-science

# Or copy template manually
cp DATA-SCIENCE-SEAL.md your-project/.claude/seal/
```

## Pre-Configured Task Types

### 1. Data Preprocessing & Cleaning

**Pattern**: Preparing data for analysis or modeling

**Initial Strategy**:
```markdown
When preprocessing data:
1. Load and inspect data
   - Check for existing data loading utilities
   - Use pd.read_csv(), pd.read_parquet(), etc.
   - Display df.info(), df.describe(), df.head()
2. Check for missing values
   - df.isnull().sum()
   - Decide strategy: drop, fill, interpolate
   - Match existing preprocessing patterns
3. Handle outliers
   - Use IQR method or Z-score
   - Check if project has outlier handling utilities
4. Encode categorical variables
   - Label encoding vs one-hot encoding
   - Check existing preprocessing pipelines
5. Scale/normalize features (if needed)
   - StandardScaler, MinMaxScaler, RobustScaler
   - Match existing feature engineering
6. Save cleaned data
   - Parquet, CSV, or pickle format
   - Follow project naming conventions
```

**Example prompts**:
- "Clean the customer_data.csv file"
- "Prepare the dataset for training"
- "Handle missing values in the sales data"

### 2. Exploratory Data Analysis (EDA)

**Pattern**: Analyzing and visualizing data

**Initial Strategy**:
```markdown
When performing EDA:
1. Load data and check structure
   - Shape, columns, data types
2. Generate summary statistics
   - df.describe(), df.value_counts()
   - Correlations: df.corr()
3. Create visualizations
   - Distribution plots: histograms, box plots
   - Relationships: scatter plots, pair plots
   - Match existing viz style (matplotlib, seaborn, plotly)
4. Identify patterns and insights
   - Trends, seasonality, anomalies
   - Feature relationships
5. Document findings
   - Create markdown cells in notebook
   - Or save plots to reports/
6. Save EDA results
   - Jupyter notebook or HTML report
```

**Example prompts**:
- "Perform EDA on the housing dataset"
- "Analyze the correlation between features"
- "Visualize the distribution of target variable"

### 3. Feature Engineering

**Pattern**: Creating and transforming features

**Initial Strategy**:
```markdown
When engineering features:
1. Understand current features
   - Domain knowledge, data dictionary
2. Create new features
   - Polynomial features, interactions
   - Date/time features: day, month, hour
   - Text features: length, word count
3. Check for existing feature transformers
   - Look in src/features/ or notebooks/
   - sklearn Pipeline or custom transformers
4. Add to feature engineering pipeline
   - Make it reproducible
   - Match existing patterns
5. Evaluate feature importance
   - Use model feature_importances_
   - Or correlation analysis
6. Document new features
   - Add to data dictionary
   - Comment why feature was created
```

**Example prompts**:
- "Create interaction features for price prediction"
- "Extract features from the date column"
- "Add polynomial features to the dataset"

### 4. Model Training & Evaluation

**Pattern**: Training ML models and evaluating performance

**Initial Strategy**:
```markdown
When training models:
1. Check existing model training code
   - Look in src/models/, notebooks/, or scripts/
   - Match train/test split patterns
2. Load preprocessed data
   - Use existing data loaders
   - Match train/val/test split ratio
3. Choose appropriate model
   - Classification: LogisticRegression, RandomForest, XGBoost
   - Regression: LinearRegression, GradientBoosting
   - Match project's preferred libraries
4. Set up cross-validation
   - KFold, StratifiedKFold, TimeSeriesSplit
   - Check existing evaluation patterns
5. Train and evaluate
   - Fit model on training data
   - Evaluate on validation/test
   - Use appropriate metrics (accuracy, F1, RMSE, etc.)
6. Save model artifacts
   - Model: joblib, pickle, or onnx
   - Metrics: JSON or CSV
   - Match existing save patterns
```

**Example prompts**:
- "Train a RandomForest classifier on the data"
- "Evaluate the XGBoost model performance"
- "Run cross-validation on the regression model"

### 5. Hyperparameter Tuning

**Pattern**: Optimizing model hyperparameters

**Initial Strategy**:
```markdown
When tuning hyperparameters:
1. Define parameter grid
   - Based on model type
   - Start with wide ranges, then narrow
2. Choose search strategy
   - GridSearchCV for small grids
   - RandomizedSearchCV for large spaces
   - Optuna for advanced optimization
   - Check what project uses
3. Set up cross-validation
   - Match existing CV strategy
   - Consider computational cost
4. Run search
   - Fit search object
   - Log progress if available
5. Evaluate best model
   - best_params_, best_score_
   - Test on held-out set
6. Save results
   - Best model and parameters
   - Search history for analysis
```

**Example prompts**:
- "Tune XGBoost hyperparameters"
- "Find best learning rate for the model"
- "Optimize RandomForest with GridSearch"

### 6. Model Deployment & Serving

**Pattern**: Deploying models to production

**Initial Strategy**:
```markdown
When deploying models:
1. Check existing deployment patterns
   - FastAPI, Flask, MLflow, BentoML
   - Look in src/api/ or deployment/
2. Save model in deployment format
   - ONNX, SavedModel, pickle
   - Match existing format
3. Create prediction endpoint
   - Input validation
   - Preprocessing pipeline
   - Model inference
   - Output formatting
4. Add error handling
   - Invalid inputs
   - Model loading failures
5. Create Docker container (if pattern exists)
   - Match existing Dockerfile
6. Write deployment docs
   - API usage examples
   - Expected input/output formats
```

**Example prompts**:
- "Create a FastAPI endpoint for the model"
- "Deploy the model with Docker"
- "Set up MLflow for model serving"

## Project Structure Detection

SEAL will auto-detect:

**Environment**:
- Python version: check `pyproject.toml`, `setup.py`, or `.python-version`
- Package manager: poetry, pip, conda

**ML Libraries**:
- scikit-learn: `sklearn` in requirements
- PyTorch: `torch` in requirements
- TensorFlow: `tensorflow` in requirements
- XGBoost/LightGBM: `xgboost`, `lightgbm` in requirements
- Pandas/NumPy: Data manipulation

**Notebook Usage**:
- Jupyter: `.ipynb` files, `jupyter` in requirements
- Kaggle notebooks: kaggle.json

**Experiment Tracking**:
- MLflow: `mlflow` in requirements, `mlruns/` directory
- Weights & Biases: `wandb` in requirements
- TensorBoard: TensorFlow project with logs/

**Project Structure**:
- Standard: `notebooks/`, `src/`, `data/`, `models/`
- Cookiecutter: Full DS project template
- Custom: Learn from existing structure

## Auto-Bootstrap Patterns

On initialization, SEAL will:

1. **Analyze notebooks** (if present):
   ```bash
   # Read first 3-5 notebooks
   # Learn:
   # - Import patterns
   # - Visualization styles
   # - Data loading approaches
   # - Model training patterns
   ```

2. **Detect data patterns**:
   ```bash
   # Check data/ directory
   # - File formats: CSV, Parquet, HDF5
   # - Naming conventions
   # - Train/test splits
   ```

3. **Learn model patterns**:
   ```bash
   # Check models/ or src/models/
   # - Model saving format
   # - Evaluation metrics
   # - Training scripts
   ```

4. **Understand experiment tracking**:
   ```bash
   # Check for MLflow, W&B, TensorBoard
   # - Logging patterns
   # - Metric names
   # - Artifact storage
   ```

## Success Metrics for Data Science

SEAL tracks:

- **Model performance**: Accuracy, RMSE, F1 trends
- **Code reproducibility**: % of experiments reproducible
- **Documentation completeness**: Notebooks with markdown explanations
- **Pipeline consistency**: % of tasks using established pipelines
- **Experiment tracking**: % of runs logged

View with:
```bash
/seal-metrics --detailed --type data-science
```

## Common Learned Patterns (Examples)

After 10+ tasks, SEAL typically learns:

### Pattern: "Train Classification Model"
```markdown
**Confidence**: 0.91 (based on 14 successful tasks)

1. Load preprocessed data from data/processed/
2. Use existing train_test_split from src/data/split.py
3. Initialize model with project defaults
4. Set up StratifiedKFold(n_splits=5)
5. Log experiment to MLflow with run_name
6. Evaluate with accuracy, precision, recall, F1
7. Save model to models/ with timestamp
8. Save metrics to metrics/<model_name>.json
9. Create confusion matrix plot
10. Log artifacts to MLflow
```

### Pattern: "EDA in Jupyter Notebook"
```markdown
**Confidence**: 0.88 (based on 11 successful tasks)

1. Create notebook in notebooks/exploratory/
2. Use standard imports (pandas, matplotlib, seaborn)
3. Load data with pd.read_parquet() (project standard)
4. Use seaborn style: sns.set_style('whitegrid')
5. Create 4-6 visualizations minimum:
   - Distribution plots for numeric features
   - Count plots for categorical
   - Correlation heatmap
   - Target variable distribution
6. Add markdown cells explaining insights
7. Save notebook with descriptive name
```

### Pattern: "Feature Engineering Pipeline"
```markdown
**Confidence**: 0.93 (based on 16 successful tasks)

1. Check src/features/ for existing transformers
2. Create transformer class inheriting from BaseEstimator, TransformerMixin
3. Implement fit() and transform() methods
4. Add to feature_pipeline in src/features/build_features.py
5. Test pipeline with sample data
6. Update data/processed/ with new features
7. Document in docs/features.md
8. Add unit tests in tests/test_features.py
```

## Integration Examples

### Example 1: Model Training

**User**: "Train an XGBoost model on the processed data"

**SEAL applies learned pattern**:
```markdown
[Reads data/processed/train.parquet]
[Checks src/models/train_model.py for XGBoost defaults]
[Sees MLflow is used for tracking]
[Creates experiment with name "xgboost-experiment-<timestamp>"]
[Trains with 5-fold CV, logs to MLflow]
[Saves model to models/xgboost_<timestamp>.pkl]
[Saves metrics to metrics/xgboost_<timestamp>.json]
```

**After 5+ similar tasks, SEAL learned**:
- Always use parquet format from data/processed/
- Log all experiments to MLflow
- Use 5-fold StratifiedKFold
- Save models with timestamp
- Include feature importance plot
- Log metrics as JSON

### Example 2: Data Preprocessing

**User**: "Clean the new customer dataset"

**SEAL applies learned pattern**:
```markdown
[Reads data/raw/customers_new.csv]
[Applies project's standard preprocessing]:
  - Drops duplicates
  - Handles missing values with median/mode
  - Encodes categoricals with LabelEncoder (project standard)
  - Scales numeric with StandardScaler
  - Saves to data/processed/customers_new.parquet
[Creates preprocessing report in reports/]
```

**SEAL learned this because**:
- 10+ previous preprocessing tasks followed this pattern
- Success rate 100% when using parquet
- User always wanted preprocessing reports

## Troubleshooting

### "SEAL isn't detecting my ML libraries"

```bash
/seal-status

# If detection failed, manually set:
# Edit .claude/seal/config.json
{
  "project_metadata": {
    "type": "data-science",
    "frameworks": ["sklearn", "xgboost", "mlflow", "jupyter"]
  }
}
```

### "Patterns don't match my workflow"

After 3-5 tasks with your workflow:
```bash
/seal-review --rating 5

# SEAL adapts to YOUR workflow
# Not generic DS patterns
```

### "Want to use different metrics"

Edit learned pattern:
```bash
# .claude/seal/learned-patterns.json
# Change metrics from ["accuracy"] to ["f1", "roc_auc"]
```

Or let SEAL learn by rating tasks highly when you use preferred metrics.

## Advanced Configuration

### Custom Evaluation Metrics

Add to `.claude/seal/config.json`:

```json
{
  "data_science": {
    "default_metrics": {
      "classification": ["accuracy", "f1", "precision", "recall", "roc_auc"],
      "regression": ["rmse", "mae", "r2"],
      "ranking": ["ndcg", "map"]
    },
    "experiment_tracking": "mlflow",
    "model_save_format": "pickle",
    "data_format": "parquet"
  }
}
```

### Auto-Experiment Tracking

Enable automatic MLflow logging:

```json
{
  "auto_log_experiments": true,
  "experiment_name_pattern": "{model_type}-{timestamp}",
  "log_artifacts": true,
  "log_models": true
}
```

SEAL will automatically:
- Start MLflow run
- Log parameters and metrics
- Save model artifacts
- End run

### Notebook Templates

Define notebook templates:

```bash
# .claude/seal/templates/eda-template.ipynb
# SEAL will use this template for EDA tasks
```

## Best Practices

1. **Reproducibility First**:
   - Set random seeds: `np.random.seed(42)`
   - Version data and models
   - Log everything to experiment tracker

2. **Follow Project Structure**:
   - data/raw/ → data/processed/
   - notebooks/ for exploration
   - src/ for production code
   - models/ for saved models

3. **Document Experiments**:
   - Use MLflow or W&B consistently
   - Add markdown to notebooks
   - Keep experiment log

4. **Test Pipelines**:
   - Unit test data preprocessing
   - Validate model outputs
   - Check for data leakage

5. **Rate Consistently**:
   - Use `/seal-review` after each task
   - Rate based on:
     - Code quality
     - Result accuracy
     - Reproducibility
     - Documentation

## Task Type Expansion

As you work, SEAL may learn these additional patterns:

- **Time Series Analysis**: ARIMA, Prophet, seasonal decomposition
- **Deep Learning**: PyTorch/TF model training, GPU usage
- **NLP Tasks**: Text preprocessing, embeddings, transformers
- **Computer Vision**: Image preprocessing, data augmentation
- **AB Testing**: Statistical tests, experiment design
- **Model Monitoring**: Drift detection, performance tracking

## Integration with MLOps

SEAL works with:

- **MLflow**: Auto-logging experiments
- **DVC**: Data versioning patterns
- **Kubeflow**: Pipeline orchestration
- **BentoML**: Model serving
- **Airflow**: Scheduled training jobs

Example MLflow integration:

```python
# SEAL learns to do this automatically:
import mlflow

with mlflow.start_run(run_name="xgboost-experiment"):
    # SEAL applies pattern
    model = train_model(X_train, y_train)
    metrics = evaluate_model(model, X_test, y_test)

    # Log automatically
    mlflow.log_params(model.get_params())
    mlflow.log_metrics(metrics)
    mlflow.sklearn.log_model(model, "model")
```

## Next Steps

1. **Initialize**: `/seal-start --template data-science`
2. **Complete 5 tasks**: Train models, preprocess data, create notebooks
3. **Review each**: `/seal-review` with ratings
4. **Watch patterns emerge**: `/seal-metrics`
5. **Enjoy faster experimentation**: 🚀

---

**Your data science workflow will accelerate with every experiment.**

**From data to deployment, SEAL learns your patterns.**
