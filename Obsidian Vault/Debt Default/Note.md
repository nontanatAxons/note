* *

kpf 
kubeflowเขียนดักถ้าเผลอกดเทรน 
@component
@pipeline -> contain the component
@
name wil define by the name of the @component
										function
? Terraform is for : set up cloud function and cloud scheduler?
? 
# pipeline

```python
build_preprocessing_args_op_train = build_train_preprocessing(

	bucket_uri=BUCKET_NAME,
	isproduction = isproduction,
	save_path=training_path,
	tsd = tsd,
	ted = ted,
	psd = psd,
	ped = ped,
	final_date = final_date,
	project_id = project_id,
	project_id_deploy = project_id_deploy
)
```

```python
build_preprocessing_args_op_test = build_test_preprocessing(

	bucket_uri=BUCKET_NAME,
	save_path=testing_path,
	solution_path=solution_path,
	isproduction = isproduction,
	tsd = tsd,
	ted = ted,
	psd = psd,
	ped = ped,
	final_date = final_date,
	project_id = project_id,
	project_id_deploy = project_id_deploy
)
```

```python
# preprocess training data

preprocess_training_spark = DataprocPySparkBatchOp(

	project=project_id,
	location=location,
	container_image=custom_container_image,
	main_python_file_uri=training_main_python_file_uri,
	runtime_config_version=dataproc_runtime_version,
	args=build_preprocessing_args_op_train.output,
	batch_id=f'training{UUID}{timestamp}',
	service_account=SERVICE_ACCOUNT,
	subnetwork_uri=SUBNETWORK_URI
)
```

```python
training_task = custom_training_component(

	bucket_uri = BUCKET_NAME,
	training_data_path = training_path,
	model_data_path=model_path,
	tsd = tsd,
	ted = ted,
	psd = psd,
	ped = ped,
	final_date = final_date,
	project_id = project_id,
	project_id_deploy = project_id_deploy,
	location = region,
	project = project_id
# base_output_directory=PIPELINE_ROOT,

)
```

```python
@component(
		   packages_to_install=["scikit-learn", "pandas", "joblib", "db-dtypes",
		   "numpy", "lightgbm", "google-cloud-bigquery", "xgboost"],
		   base_image="python:3.9",
		   output_component_file="pipeline/training.yaml",)

def training(

	bucket_uri: str,
	training_data_path: str,
	model_data_path: str,
	output_model_artifact: Output[Artifact],
	tsd: str,
	ted: str,
	psd: str,
	ped: str,
	final_date: str,
	project_id: str,
	project_id_deploy: str,

):
```

```python
custom_training_component = utils.create_custom_training_job_op_from_component(
	
	# custom_train_model, replica_count=1
	training,
	display_name = 'training-n1-highmem-8',
	machine_type = 'n1-highmem-8',
	# accelerator_type='Nvidia Tesla P100 GPU',
	# accelerator_count='1'
)
```

```python
	training_task.after(preprocess_training_spark) # training pipeline
	predicting_task.after(training_task).after(preprocess_testing_spark) #predict pipeline
```

create_custom_training_job_op_from_component -> create the Kubeflow pipeline component
DataprocPySparkBatchOp -> create the Kubeflow pipeline component

ส่งข้อมูลไป 2 ที่ คือ bucket for tracking version and to bigQ for customer

spark, dataproc
lag 24 ถอยหลังไป 24 times

window

lag = 24 is go back 24 week from ted train end date

pipe1 predict
pipe2 show previous actual pipe3 show previous actual just additional feature

p May, log askable

-------------------------


![[Screenshot from 2023-09-15 17-12-04.png]]
tsd = train start date

** window preprocessing
future improvement : 
- look at the feature extraction code more
