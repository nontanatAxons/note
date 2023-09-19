- ML flow (python pakage)
tracking include
	- scource code
	- environment
	- Data
	- Model
	- Hyper param
	- metric
	
To start the project -> create the new env is good idea instead of install everything locally
- `conda create -n <name-project> python=3.9`
- `conda activate <name-project>`

then `pip install the requirements.txt` and then do the following command

`mlflow ui --backend-store-uri sqlite:///mlflow.db`

- Experiment tracking with MLflow
- Saving and loading models with MLflow

`mlflow.xgboost.autolog(disable=True)` # means the mlflow not gonna track the parameter autometically 

## experiment logging

### experiment logging
```python
with mlflow.start_run():
	mlflow.log_params() # Experiment trackingmlflow.log_metric()` # Experiment tracking
```

### Model management
```python
	
## model management (# save pre-processing and the model in the artifact in mlflow)

mlflow.log_artifact("models/preprocessor.b",artifact_path="preprocessor") # save preprocessing because it's a good idea to also save the preprocessing (single file to load)

mlflow.xgboost.log_model(booster,artifact_path="models_mlflow") # 
```
#### save model in many file including
 - detail, 
 - conda.yaml (incase of using the conda env),
 - model.xgb, (xgb object model)
 - requirement.txt (if use the local env)
 
 Read the logged model back:
 after artifact the model it's will provide in the mlflow ui the uri to load the model from ml flow
 
 We can read the model back using python flavour or the paticular model flavor.

# Model Registry

![[Screenshot from 2023-09-07 18-51-28 1.png]]

Just label the assignment to the model (with cicd code) 
![[Screenshot from 2023-09-07 18-55-53.png]]
just click the button

![[Screenshot from 2023-09-07 18-58-36.png]]


# MLflow in practice

#### senario 1: artfac and logging the model locally

#### senario 2: Cross functional team

```
mlflow server --backend-store-uri sqlite:///backend.db
```
this will create the file backend.db locally that will store the mlflow logging and model that will push to the mlflow ui server

but if
```
mlflow server --backend-store-uri sqlite:///backend.db --default-artifact-foot ./artifacts_local
```
this will also create the local repo that store the same thing as in backend.db but this is store in the local folder 

#### Scenario 3: Multiple data scientists working on multiple ML models

MLflow setup:

* Tracking server: yes, remote server (EC2).

* Backend store: postgresql database.

* Artifacts store: s3 bucket.

  

The experiments can be explored by accessing the remote server.

  

The exampe uses AWS to host a remote server. In order to run the example you'll need an AWS account. Follow the steps described in the file `mlflow_on_aws.md` to create a new AWS account and launch the tracking server.

everything is the same excwpt that the mlflow can be shared with others by upload it into aws s3 bucket (like a web)![[Screenshot from 2023-09-07 23-53-21.png]]
