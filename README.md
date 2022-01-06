# Udacity-MLOPS-Capstone-Project
Udacity Azume ML engineer Nanodegree capstone project

[![standard-readme compliant](https://img.shields.io/badge/readme%20style-standard-brightgreen.svg?style=flat-square)](https://github.com/RichardLitt/standard-readme)


## Table of Contents

- [Project Overview](#Project-Overview)
- [Dataset](#Dataset)
- [Architecture Diagram](#Architecture)
- [Generator](#generator)
- [Badge](#badge)
- [Example Readmes](#example-readmes)
- [Related Efforts](#related-efforts)
- [Maintainers](#maintainers)
- [Contributing](#contributing)
- [License](#license)

<a name="Project-Overview"/>

## Project Overview

In this capstone project for Udacity Azure ML Nanodegree, I had to bring in my own dataset , define the problem and build two models using (1) Azure AutoML (2) Custom Scikitlearn model using HyperDrive. Both the models used Jupyter Notebook as control plane (using SDK) for the entire process i.e creating experiment,attaching compute cluster, creating pandas dataframe,setting up AutoMl or HyperDrive configuration, Picking the best model, deployment and consumption as an API. All steps are explained in detail below.  

The Datset used is Bank Personal Loan and trying to predict if a customer would apply for loan or not. Using this model Bank can offer personal loans to prospective customer. Dataset detail shown below.


## Dataset

This project uses [Bank_Personal_Loan](https://raw.githubusercontent.com/bhattajo/Udacity-MLOPS-Capstone-Project/main/Bank_Personal_Loan_Modelling.csv) dataset. the dataset contains these columns:

1.  ID : ID of customer.
2.  Age : Age of customer.
3.  Experience : Work experience in years of customer.
4.  Income : Income of customer (This is scaled down to $income/1000).
5.  Zip Code : Zip code of customer.
6.  Family : Number of family member/s of customer including him.
7.  CCAvg : Credit Card balance (average) of customer
8.  Education : Education of customer (Encoded)
9.  Mortgage : Mortage or not of customer (0=no, 1=yes)
10. Personal Loan : Whether customer took personal loan or not (0=no, 1=yes). This is the independent variable (**Prediction column**).
11. Securities Account : Whether customer has Securities Account (0=no, 1=yes).
12. CD Account : Whether customer has CD Account  (0=no, 1=yes).
13. Online : Whether customer has online account  (0=no, 1=yes).
14. CreditCard : Whether customer has credit card account  (0=no, 1=yes).

![image](https://user-images.githubusercontent.com/19474037/148328927-3cf92161-3594-459d-9830-654afe98a06b.png)


## Architecture

Image:1&2 Shown below how an AutoML run can be triggered by feeding the three main components i.e Dataset, Optimization Metrics and Constraints. In detail section below, the code snipped is shared for your perusal.

Image:2 Shown below explains the overall architecture on how to use SDK to trigger an experiment (Both AutoML and HyperDrive with cutom code) in Azure ML studio, pick the best model for deployment (on Azure Container Instance), generate application insight/Swagger documentation and consumption using REST call.

![image](https://user-images.githubusercontent.com/19474037/148326105-73f80f5c-c110-4b07-8fd9-28183d3307b3.png)
<p align="center">Image:1</p>

![image](https://user-images.githubusercontent.com/19474037/148414152-188e2821-1a38-4d3a-b55e-711875606cf7.png)
<p align="center">Image:2 (- Azure AutoML workflow taken from the official infographic)</p>

![image](https://user-images.githubusercontent.com/19474037/148328370-fd91a462-9a6c-48de-a2e7-be3bf184f90a.png)
<p align="center">Image:3</p>


## Setup

1. Setup Notebook environment in Azure ML Sudio
  - Copy **train.py** from starter file
  - copy 2 notebboks **automl** and **hyperparameter_tuning** from starter file

2. Setup local notebbok environment with Azure ML sdk downloaded using pip.

## AutoML

AutoML uses Bayesian optimization to identify better hyper-parameters than human experts. It also uses collaborative filtering (Probabilistic Matrix Factorization) to search for the most promising data transformation pipelines efficiently (Luca Zavarella, 2020). 

Matrix factorization is a technique used where awe have combinatorial explosion. It uses features to decompse a big combination set into smaller factor set. Think of it like how  factor a big number for ex. 12 into factors of 2 small numbers (3X4 and 6X2)

For AutoML experiment. We first create workspace from config (ws = Workspace.from_config()) and create a Panda Dataframe using the Tabular Dataset.

![image](https://user-images.githubusercontent.com/19474037/148329351-29a630d3-aaf4-475a-8691-f410d61578d2.png)

AutoAMLConfig shown below with all the settings including primary metric as AUC_Weighted and early stopping using the dataset retrieved from above step. The label_column is Personal Loan (Dependent variable). This is what the model will predict after it learns from the data.

![image](https://user-images.githubusercontent.com/19474037/148329446-bcc3e968-b608-4018-86f6-96876be299ad.png)

The RunDetails are shown below for the AutoML Run.
![image](https://user-images.githubusercontent.com/19474037/148331107-83ac1eac-6a24-42a2-bdc6-0c2290200169.png)

***Experiment Details from Azure ML Studio:***

We can login to Azure ML studio and see the experiment run in more detail. It shows the duration of the run, recall score (weighted) etc.

![image](https://user-images.githubusercontent.com/19474037/148331372-eb18b07b-83ce-4d7e-9f90-8ec0afddd60d.png)

***Best Model is VotingEnsemble with a surpringly high accuracy 💌:***

In the model section, we can see all the runs and the best model at the top. The voting classifier works like an electoral system in which a prediction on a new data point is made based on a voting system of the members of a group of machine learning models (Samson Afolabi, 2020).

![image](https://user-images.githubusercontent.com/19474037/148331554-71e061af-3d9a-476c-9cfe-be987f493227.png)
![image](https://user-images.githubusercontent.com/19474037/148332363-ad3ee42f-1a76-47ce-9453-97cdb7e893a7.png)
![image](https://user-images.githubusercontent.com/19474037/148332417-3deba64b-3a90-4196-bb0d-26d946cea6b4.png)


***Metrics:***

The Metrics tab in AzureML studio gives ability to view various metrics using chart or a table. It provides ability to also see various additional metrics like confusion matrix and Recall. 

![image](https://user-images.githubusercontent.com/19474037/148332794-3d9ddafb-e215-4073-ae16-f5c296071f2e.png)

***Explanation:***

The Metrics tab in AzureML studio gives ability to see model explaniability (XAI). Information like feature importance bar chart is shown below. This can be used to clean the dataset and apply feature engineering to improve the model performance.

![image](https://user-images.githubusercontent.com/19474037/148332860-ba2eb798-3b84-4b96-91be-721790ede918.png)
![image](https://user-images.githubusercontent.com/19474037/148417948-08543179-0f6d-4adb-ad4d-9fcc96c5f305.png)
![image](https://user-images.githubusercontent.com/19474037/148418385-b4dce0b7-c5b4-42bc-b76b-d74a23fbce91.png)


***Saving the Best Model:***

The best model from AutoML run is picked up and saved locally. By default AutoML creates a model.pkl file in /outputs folder. Once the model is saved we can reuse it without the need for lenghty training step. Shown below code for downloading the model:

![image](https://user-images.githubusercontent.com/19474037/148420594-1f4121cf-6427-4d9d-ae8d-2766323a43bb.png)

Here is the AzureML screenshot of model.pkl generated by AutoML run:

![image](https://user-images.githubusercontent.com/19474037/148420772-2fa340c6-7cf7-44ea-83e5-1849f03e2331.png)

***Model Registration and Deployment:***

Finally the best run model is registered and then deployed* in Azure ML studio creating endpoints, swagger file. Using the auth_enabled=True will create authorization primary and secondary keys and will protect the endpoint from attack. However if you are planning to run a pipeline and test it in that, you dont need one and the ephemeral endpoint can be deleted after the run.

![image](https://user-images.githubusercontent.com/19474037/148422035-fc9e9c66-fc55-448b-8bf5-ae2580a657b3.png)

*deployment of Automl run was done after running the HyperDrive model and selecting the best model between the 2 experiments.

Azure ML studio Endpoints tab showing the published model:

![image](https://user-images.githubusercontent.com/19474037/148422220-5f447349-f5a8-4eb3-a6d8-266ad4db2b1f.png)

## Hyperparameter training using Hyperdrive

Hyperparameter tuning is choosing a set of optimal hyperparameters for a Machine learning algorithm. All Machine learning libraries (scikit learn) allows attributes to be set before the learning process begins. An optimal set of these attribute values gives high accuracy and the model performs better. Due to large number of these attributes and combinatorial explosion Hyperparameter can be a costly and long run. There are techniques like Random/Grid and Bayesian sampling to optimize the run. Its a delicate dance between Big O and hyperparameter optimization. If given infinite compute and time, a continuous search space can be used to gurantee the best optimal values.

***Hyperdrive Configuration***

In this project (hyperparameter_tuning.jpynb) i used Bandit Policy for early termination and two hyper parameters supported by scikitlearn logistic regression model. These are regularization and max iter. Regularization like Ridge adds a penalty term with a facor to prevent model from overfiting. Max iter controls how many steps the algorithm will take in the gradient descent before giving up. This helps prevent Gradient Decent divergence as shown below:

![image](https://user-images.githubusercontent.com/19474037/148430631-620b5aa5-995d-4305-ac42-3c19d0fb196d.png)

Code showing HyperDrive configuration like "--C" for regularization and "--max_iter" for maximum iteration:

![image](https://user-images.githubusercontent.com/19474037/148431001-219699bf-f24f-4ff2-8793-47ce435a8778.png)

The ScriptRunConfig class is set with train.py as script. The python script train.py contains code for feature engineering (like scaling and one hot encoding) , splitting the dataset into train/test set and running the Logistic Regression using scikitlearn package. The hyperDrive config passes RandomParameter sampling data ("--C" and "--max_iter") and creates run for each of these as child models. RunDetail widget of Hyperdrive run inside notebbok is shown below:

![image](https://user-images.githubusercontent.com/19474037/148432715-6daf95d3-c366-4663-acc9-aaa1cf653e9a.png)
![image](https://user-images.githubusercontent.com/19474037/148432837-6b1034c5-ba20-422a-9ed4-7315bb36f354.png)

RunDetail from Azure ML Studio Experiment tab:

![image](https://user-images.githubusercontent.com/19474037/148433764-451a1fb6-de44-42a8-99e2-bff11fa3ecb2.png)

***Best Model***

![image](https://user-images.githubusercontent.com/19474037/148434051-ecc0eca9-af73-448c-a18b-0f2a7e1c9083.png)

Azure ML studio Experiment tab data:

![image](https://user-images.githubusercontent.com/19474037/148434678-cfdd4d0b-edcf-4aa0-8740-82b0dd4e7ece.png)

***Saving and Registering and the best Model***
![image](https://user-images.githubusercontent.com/19474037/148435200-3c674274-b8e9-45b5-b36b-dcff23f1fcf0.png)

Model saved locally in Azure ML studio worspace:

![image](https://user-images.githubusercontent.com/19474037/148435398-03bc2fbf-a256-46ee-86cf-c1727dcbf62f.png)

Model Registered in Azure ML:

![image](https://user-images.githubusercontent.com/19474037/148435782-158e37f5-36ae-492d-9fde-f267c5390366.png)

## Model Consumption

***Consumption from the notebook code***
![image](https://user-images.githubusercontent.com/19474037/148436941-fad69e42-842a-45ee-9f14-077315704c23.png)


***Consumption from python script endpoint.py***

![image](https://user-images.githubusercontent.com/19474037/148437568-8cc024ba-08ba-414c-ac5a-39e4734fdd7b.png)


## Maintainers

[@bhattajo](https://github.com/bhattajo).

## Contributing

Feel free to dive in! [Open an issue](https://github.com/bhattajo/standard-readme/issues/new) or submit PRs.

Standard Readme follows the [Contributor Covenant](http://contributor-covenant.org/version/1/3/0/) Code of Conduct.


## References
<a id="1">[1]</a> 
**AciWebservice Class** Microsoft (2021). 
https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-cloud/model-register-and-deploy.ipynb
