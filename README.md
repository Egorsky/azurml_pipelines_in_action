# azurm_pipelines_in_action
This repository contains code for ensemble model deployment.

# Data

Data for this example is synthetic and contains of columns representing date, sales and complaints.

# Models

Considering synthetic data is based on simple `sine`, `cosine` functions, `random noise` and holidays in Poland it is not really represents any behaviour of the product. Therefore all decisions upon models are made in order to provide an example.

For sales forecasting SARIMA model provided by `statsmodels` is used. The out of sample forecasting is created with help of predicted mean. More information about the model may be found [here](https://www.statsmodels.org/dev/generated/statsmodels.tsa.statespace.sarimax.SARIMAX.html).
For complaints forecasting prophet model is used. You may read more about this model in this [documentation](https://facebook.github.io/prophet/docs/quick_start.html).
All hyperparameters for the model are based on EDA. Optuna was used to get hyperparameters for the Prophet model. More information about Optuna may be found [here](https://optuna.readthedocs.io/en/stable/).

# Pipeline

The pipeline contains of 5 components:
- SARIMA model training. In this component the data is gathered directly froim the Azure Blob storage using `SecretClient` for accessing to the SAS Token.
- Model forecasting. Reads model and data from the previous step, makes forecast and creates extended `DataFrame`
- Feature creation. Here feautres like lags and movig average are created
- Prophet model training. Component gets data with features from the previous steps and trains model
- Forecasting of the complaints. The component gets model and data from the previous component and makes forecast which further on gets uploaded into the Blob Storage Conatiner as a `csv` file.

# Features & Artifacts

Througut the pipeline feaures get lщgged via the MLFlow. We also record plots and models which allow to keep version control of the model and track its performance.
