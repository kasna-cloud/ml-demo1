# ml-demo1

This repo contains the code for the Kasna/Eliiza Google ML Specialisation, demo 1.  For this example we have chosen to use an existing public example of Machine Learning on GCP. 


## Use-Case

The use-cawse is an example of how to detect anomalies in financial, technical indicators by modeling their expected distribution and thus inform when the Relative Strength Indicator (RSI) is unreliable. RSI is a popular indicator for traders of financial assets, and it can be helpful to understand when it is reliable or not. This example will show how to implement a RSI model using realistic foreign exchange market data, Google Cloud Platform and the Dataflow time-series sample library.

## Success Criteria
### Code
#### Code Repository

The public source repository is availble on the [Kasna Cloud github page](https://github.com/kasna-cloud/dataflow-fsi-example).
This repo contains all of the source, infrastructure build and run tools for the example as well as information on the [creation and deployment of the ML model](https://github.com/kasna-cloud/dataflow-fsi-example/tree/main/notebooks).

Detailed documentation and a blog are also available in the [docs](https://github.com/kasna-cloud/dataflow-fsi-example/tree/main/docs) folder of the repo.

#### Code origin certification
Kasna certifies that all code within this repository is original and developed by Kasna/Eliiza and licensned under the MIT open-source license as defined [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/LICENSE).

All libraries and open-sourced code used in this project has not been modified and is used under the relevant library/code license.

### Data
#### Dataset in GCP
This project utilises a data generator to create realistic foreign exchange price pairs. Implementation details of this data generator are available within the [forexgenerator.py](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/app/python/src/forexgenerator/forexgenerator.py) program. During deployment of the project, this generator is run from a GKE cluster and scaled to generate the desired pair volumes.

### Whitepaper/Blog post
#### Business Goal and ML Solution

The Relative Strength Index, or RSI, is a popular financial technical indicator that measures the magnitude of recent price changes to evaluate whether an asset is currently overbought or oversold.

The general approach regarding the RSI is to be used as a mean-reversion trading strategy:

* When the RSI for an asset is less than 30, traders should buy this asset as the price is expected to increase in the near future.
* When the RSI for an asset is greater than 70, traders should sell this asset as the price is expected to decrease in the near future.
As a simplified rule, this strategy cannot be trusted to work favorably at all times. Therefore in these instances (i.e. RSI > 70 or RSI less than 30), it would be useful to know if the RSI is a reliable indicator to inform a trade or not.

The full blog/whitepaper which details this use-case can be found in the project repo [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/BLOG.md).

In addition, Google published another version which was created by Kasna/Eliiza and is available on the [Google website](https://cloud.google.com/blog/topics/financial-services/detect-anomalies-in-real-time-forex-data-with-ml). 

#### Data Exploration
Data exploration and discovery for this project is explained in the Jyupyter notebook available [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_data_exploration.ipynb). This notebook explains how and what type of data exploration was performed and what decisions were influenced by data exploration for model creation.

#### Feature Engineering
Feature engineering for the LSTM model is detailed within the two Jupyter notebooks used in the project. These are:
* [Data Exploration notebook](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_data_exploration.ipynb)
* [TFX Training Pipeline](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb)

#### Preprocessing and the data pipeline
The TFX pipline code is within the repository source and explained in the [TFX Training Pipeline](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb) notebook and the pipeline components are described [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/COMPONENTS.md).

This documentation includes the data preprocessing pipeline, training and model serving.

#### ML Model Desgin(s) and Selection


Partners must describe the following:
* Which ML model/algorithm(s) were chosen for demo #1?
* What criteria were used for ML model selection?

*Evidence* must describe (in the Whitepaper) selection criteria implemented, as well as the specific ML model algorithms that were selected for training and evaluation purposes. Code snippets detailing the model design and selection steps must be enumerated.

#### ML model training and development
Partners must document the use of Cloud AI Platform or Kubeflow for ML model training, and describe the following:
* Dataset sampling used for model training (and for independent dev/test datasets) and justification of sampling methods.
* Implementation of model training, including adherence to GCP best practices for distribution, device usage, and monitoring.
* The model evaluation metric that is implemented, and a discussion of why the implemented metric is optimal given the business question/goal being addressed.
* Hyper-parameter tuning and model performance optimization.
* How bias/variance were determined (from the train-dev datasets) and tradeoffs used to influence and optimize ML model architecture?

*Evidence* must describe (in the Whitepaper) each of the ML model training and development bullet points (above). In addition, code snippets that accomplish each of these tasks need to be enumerated.

#### ML Model Evaluation
Partners must describe how the ML model, post-training, and architectural/hyperparameter optimization performs on an independent test dataset.

*Evidence* must include records/data (in the Whitepaper) of how the ML model developed and selected to address the business question performance on an independent test dataset (that reflects the distribution of data that the ML model is expected to encounter in a production environment). In addition, code snippets on model testing need to be enumerated.

### Proof of Deployment
#### Model application on GCP 
Partners must provide proof that the ML model/application is deployed and served on GCP with Cloud AI Platform or Kubeflow.

*Evidence* must include the Project Name and Project ID of the deployed cloud machine learning model and client.

#### Callable library/application
Partners must demonstrate that the ML model for demo #1 is a callable library and/or application.

*Evidence* must include a demonstration of how the served model can be used to make a prediction via an API call.

#### Editable model/application
Partners must demonstrate that the deployed model is customizable.

*Evidence* must include a demonstration that the deployed model is fully functional after an appropriate code modification, as might be performed by a customer.







