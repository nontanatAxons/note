	1. st Do the experiment on notebook
	2.  
	3. ML pipeline (ml_pipeline.py) output of the pipe line is the model
	4.  Then the model will be deploy in yhe webservice + add(monitoring of the performance)
	
To automate everything

4 Level

## lv0: No MLops
	No Automation
	all code in jupyter

	Good for -> POC level

## lv1: Devops, No MLops
(+)
	- release are automate
	- unit and integration test
	- CI/CD
	- ops metrics
	
(-)
	- no experiment tracking
	- no reproductibility
	- DS seperate from engineer
	
## lv2: Automated Training (2-3 model cases)
(+)
	- training pipeline
	- experiment tracking
	- model registry
	- low friction deploment
	- DS work with engineer
	
## lv3: Automated Deployment
	- Easy to deploy model
	- model monitoring
	
## lv4: Full MLops Automation (Auto retrain, deploy)

Not every case go to level 3,4 (most of them go to level 2)

This course for model number 1-2 and some of number 3
