# ml-demo1

This repo contains the code for the Kasna/Eliiza Google ML Specialisation, demo 1.  For this example we have chosen to use an existing public example of Machine Learning on GCP. 

## 2 Use-Case

The use-cawse is an example of how to detect anomalies in financial, technical indicators by modeling their expected distribution and thus inform when the Relative Strength Indicator (RSI) is unreliable. RSI is a popular indicator for traders of financial assets, and it can be helpful to understand when it is reliable or not. This example will show how to implement a RSI model using realistic foreign exchange market data, Google Cloud Platform and the Dataflow time-series sample library.

## 3.1 Success Criteria
### 3.1.1 Code
---

#### 3.1.1.1 Code Repository

The public source repository is availble on the [Kasna Cloud github page](https://github.com/kasna-cloud/dataflow-fsi-example).
This repo contains all of the source, infrastructure build and run tools for the example as well as information on the [creation and deployment of the ML model](https://github.com/kasna-cloud/dataflow-fsi-example/tree/main/notebooks).

Detailed documentation and a blog are also available in the [docs](https://github.com/kasna-cloud/dataflow-fsi-example/tree/main/docs) folder of the repo.

#### 3.1.1.2 Code origin certification
Kasna certifies that all code within this repository is original and developed by Kasna/Eliiza and licensned under the MIT open-source license as defined [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/LICENSE).

All libraries and open-sourced code used in this project has not been modified and is used under the relevant library/code license.

### 3.1.2 Data
---

#### 3.1.2.1 Dataset in GCP
This project utilises a data generator to create realistic foreign exchange price pairs. Implementation details of this data generator are available within the [forexgenerator.py](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/app/python/src/forexgenerator/forexgenerator.py) program. During deployment of the project, this generator is run from a GKE cluster and scaled to generate the desired pair volumes.

### 3.1.3 Whitepaper/Blog post
---

#### 3.1.3.1 Business Goal and ML Solution

The Relative Strength Index, or RSI, is a popular financial technical indicator that measures the magnitude of recent price changes to evaluate whether an asset is currently overbought or oversold.

The general approach regarding the RSI is to be used as a mean-reversion trading strategy:

* When the RSI for an asset is less than 30, traders should buy this asset as the price is expected to increase in the near future.
* When the RSI for an asset is greater than 70, traders should sell this asset as the price is expected to decrease in the near future.
As a simplified rule, this strategy cannot be trusted to work favorably at all times. Therefore in these instances (i.e. RSI > 70 or RSI less than 30), it would be useful to know if the RSI is a reliable indicator to inform a trade or not.

The full blog/whitepaper which details this use-case can be found in the project repo [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/BLOG.md).

Additionally, Google published another version which was created by Kasna/Eliiza and is available on the [Google website](https://cloud.google.com/blog/topics/financial-services/detect-anomalies-in-real-time-forex-data-with-ml). 

#### 3.1.3.2 Data Exploration
Data exploration and discovery for this project is explained in the Jyupyter notebook available [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_data_exploration.ipynb). This notebook explains how and what type of data exploration was performed and what decisions were influenced by data exploration for model creation.

#### 3.1.3.3 Feature Engineering
Feature engineering for the LSTM model is detailed within the two Jupyter notebooks used in the project. These are:
* [Data Exploration notebook](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_data_exploration.ipynb)
* [TFX Training Pipeline](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb)

#### 3.1.3.4 Preprocessing and the data pipeline
The TFX pipline code is within the repository source and explained in the [TFX Training Pipeline](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb) notebook and the pipeline components are described [here](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/COMPONENTS.md).

This documentation includes the data preprocessing pipeline, training and model serving.

#### 3.1.3.5 ML Model Desgin(s) and Selection
Model selection is detailed within the [Data Exploration notebook](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_data_exploration.ipynb) as well as the 
[blog](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/BLOG.md).

#### 3.1.3.6 ML model training and development
The ML model training is described in the [TFX Training Pipeline](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb) notebook as well as the [blog](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/BLOG.md).
These documents include the following aspects of training:
* Dataset sampling used for model training (and for independent dev/test datasets) and justification of sampling methods. [link](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/BLOG.md#training-the-model-in-tensorflow-extended-tfx-using-dataflow-kubernetes-engine-and-ai-platform)
* Implementation of model training, including adherence to GCP best practices for distribution, device usage, and monitoring. [link](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/BLOG.md#training-the-model-in-tensorflow-extended-tfx-using-dataflow-kubernetes-engine-and-ai-platform)
* The model evaluation metric that is implemented, and a discussion of why the implemented metric is optimal given the business question/goal being addressed.
    > The TFX Trainer component then trains the model by minimising the loss function: mean squared error between the input features and model output. The model’s evaluation metric is also the mean squared error.
    > We experimented with different model window sizes to minimise the evaluation metric. You can also experiment with different model window sizes by changing LSTM_WINDOW_LENGTH in config.sh. This value represents how much time we are looking across to detect anomalies. So for example, with a window size of 5 elements and Dataflow sample library parameter METRICS_TYPE_1_WINDOW_SECS = 1, each window represents 5 seconds of metrics. The table below contains the summarised details, you can see that the model achieves the best (lowest) evaluation metric with a window size of 30. 
* Hyper-parameter tuning and model performance optimization. [link](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb)
    > Here we manually run the trained model on the evaluation dataset and compute the reconstruction error (RE). Anomalies are determined by comparing RE to a threshold. The threshold is selected by assuming a target anomaly rate of 10%. The anomalies are then plotted.
    > Note that we must evaluate the model in this way because the TFX Evaluator component cannot be used as it currently does not handle model output variables created in the Transform component, which is a requirement for autoencoder models.
* How bias/variance were determined (from the train-dev datasets) and tradeoffs used to influence and optimize ML model architecture? [link](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb)

#### 3.1.3.7 ML Model Evaluation
The ML model, re-training and evaluation are described in the notebook [TFX Training Pipeline](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/notebooks/example_tfx_training_pipeline.ipynb)

### 3.1.4 Proof of Deployment
---

#### 3.1.4.1 Model application on GCP 
The repo contains all of the scripting and tools required to deploy to GCP.

#### 3.1.4.2 Callable library/application
The model is deployed to AI Platform and served as per the [FLOW diagram](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/docs/FLOWS.md) 

#### 3.1.4.3 Editable model/application
Any of the model parameters can be changed within the source code [trainer.py](https://github.com/kasna-cloud/dataflow-fsi-example/blob/main/app/python/src/tfx_components/trainer.py)

