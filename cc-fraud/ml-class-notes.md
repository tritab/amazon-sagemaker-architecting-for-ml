# AWS ML Class

Emily Webber - egwebber@amazon.com

## What is modeling

* Use case
* Data
* Model

## Modeling

* Logistic model (sigmoid function) takes an input and outputs [0,1]
* We must validate the model to have confidence in the output (e.g. AUC <60% may not be sufficient)
* Compare Y^ with the actual. Diffence is the loss function
* Model learns through SGD (minimize the loss function). High school calculus

## Sagemaker

* Notebook instance
* 17 built-in algos
  * proprietary - in some cases whitepapers are available
* ML Training Service 
  * can run other algos from containers stored in ECR as a managed service. Can close notebook
  * requires data to be in S3
* Hyper-parameter tuning service (3 x 20 is good starting point)
* ML Hosting Service 
  * deploy the model as a REST api
  * dedicated endpoint as an EC2 instance
  * good for online inferencing
  * api gateway -> lambda -> endpoint
  * What differentiates sagemaker hosting from other AWS hosting?
* Batch transform
  * Inferncing and data transformation
* SDKs
  * Python (boto3)
  * Spark
* Documentation
* Experiment management - track options and outcomes
* Workflow automation
* Pipeline models
* Git repo
* AWS Optimized tensorflow 50% speedup
* Elastic Inference - designed to save cost issues with deployed endpoints

## Algorithms

* Many support distributed training 
  * linear learner, unsupervised, seq2seq, kmeans, random cut forests, IP insights
* Some support incremental training
  * image classification, object detection
* Can also bring your own algos
* Also support code in their container
  * then can use their web server
* Model artifact can be exported and used outside sagemaker. Would need to build interface.

## WHen to bring your own model

* Learning
* New problem
* Expertise

## Sagemaker Ground Truth

* Mechanical Turk
* Private Labeling Workforce (gui labeling tool)
* 3rd party vendors

## Marketplace for ML

* Algos and models from 150+ vendors
* H2o.ai automl is there

## Sagemaker Neo

* Train once, run anywhere
* Neo shrinks model 10X. is open source
* optimize for intel, nvidia, arm, etc.
* https://aws.amazon.com/sagemaker/neo/

## Reinforcement Learning

* simulations, scoring function, RL algo
* use cases
  * supply chain simulation, manufacturing process, robot manipulation, autonomous car
* AWS robomaker and deepracer

## AWS ML Cert

* Beta available now
* This class is a good prep for it
* ML University

## Best practices

* Will typically create accounts per project
* Dont have 40 people in 1 account
* Default notebooks is around 20
* There is no SSH access to the Sagemaker notebook ec2 instance, it is a managed service
* Sagemaker notebook instances will not show up on the ec2 console

## Sagemaker usage

* can create lifecycle config, to install packages, copy data, etc
* lifecycle config can be applied to notebooks when we create it
* use python multiprocessing (Pool) to greatly speed up jobs to scale to the # of CPUs
* https://aws.amazon.com/blogs/machine-learning/call-an-amazon-sagemaker-model-endpoint-using-amazon-api-gateway-and-aws-lambda/
* Interesting that the notebook instance only runs the notebook. The actual training happens in a separate training instance.

## Modeling approach

* sometimes tier the models
  * fraud or not
  * type of fraud

## Chicago crime

* https://github.com/aws-samples/amazon-sagemaker-architecting-for-ml/tree/master/Example-Project

## Monday started lab

* Fraud detection

## Tuesday

## Feature engineering

* What rows and columms do we already have?
* Do I need to transform any columns? Normalize, standardize
* Deep learning models need inputs scaled in 0-1.
  * scikit learn min-max scaler
  * subtract by the mean then divide by the standard deviation
* remove outliers
* likely need one hot encoding

## Model evaluation

* Confustion matrix
  * True pos and true negative are good
  * False neg - aka recall (# of true positives over the number of samples)
  * False positive - Precision (# of true positives over the number of positive predictions)
  * accuracy = (tp + tn) / total predictions
  * precision = TP / positive predictions
  * recall = TP / Positive samples
  * Receiver Operator Curve (ROC)
  * AUC is the area under the ROC
  * https://aws.amazon.com/blogs/machine-learning/training-models-with-unequal-economic-error-costs-using-amazon-sagemaker/
  * Deep learning with Python book (keras)

## Hyperparameter Tuning

* Reviewed sagemaker example hpo_xgboost_direct_marketing_sagemaker_python_sdk notebook
* See output of tuning job in HPO_analyze_tuningjob_results notebook
* Works for all built-in and byo algo

## Going to production

* Separate AWS accounts for prod vs Data scientists
* How to handle model performance - take offline with Emily
* Endpoint, lower latency for real time predictions
* Batch - cheaper, but minutes of spin up time
* Or, take sagemaker model, run in lambda, need to write own inference code

## Architecture

* http://www.shreyasdemos.ml/sports-replay-generation-using-ml/
* this example uses too many AWS products since the person who did it wanted experience, for a real example, we could greatly simplify it
* Step functions to orchestrate lambdas
