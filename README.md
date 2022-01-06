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

Image:1 Shown below how an AutoML run can be triggered by feeding the three main components i.e Dataset, Optimization Metrics and Constraints. In detail section below, the code snipped is shared for your perusal.

Image:2 Shown below explains the overall architecture on how to use SDK to trigger an experiment (Both AutoML and HyperDrive with cutom code) in Azure ML studio, pick the best model for deployment (on Azure Container Instance), generate application insight/Swagger documentation and consumption using REST call.

![image](https://user-images.githubusercontent.com/19474037/148326105-73f80f5c-c110-4b07-8fd9-28183d3307b3.png)
                                                Image:1
![image](https://user-images.githubusercontent.com/19474037/148328370-fd91a462-9a6c-48de-a2e7-be3bf184f90a.png)
                                                Image:2


## Setup

1. Setup Notebook environment in Azure ML Sudio
  - Copy **train.py** from starter file
  - copy 2 notebboks **automl** and **hyperparameter_tuning** from starter file

2. Setup local notebbok environment with Azure ML sdk downloaded using pip.

## AutoML Notebook

For AutoML experiment. We first create workspace from config (ws = Workspace.from_config()) and create a Panda Dataframe using the Tabular Dataset.

![image](https://user-images.githubusercontent.com/19474037/148329351-29a630d3-aaf4-475a-8691-f410d61578d2.png)

AutoAMLConfig shown below with all the settings including primary metric as AUC_Weighted and early stopping using the dataset retrieved from above step. The label_column is Personal Loan (Dependent variable). This is what the model will predict after it learns from the data.

![image](https://user-images.githubusercontent.com/19474037/148329446-bcc3e968-b608-4018-86f6-96876be299ad.png)

The RunDetails are shown below for the AutoML Run.
![image](https://user-images.githubusercontent.com/19474037/148331107-83ac1eac-6a24-42a2-bdc6-0c2290200169.png)

### Experiment Details from Azure ML Studio

We can login to Azure ML studio and see the experiment run in more detail. It shows the duration of the run, recall score (weighted) etc.

![image](https://user-images.githubusercontent.com/19474037/148331372-eb18b07b-83ce-4d7e-9f90-8ec0afddd60d.png)

### Best Model is VotingEnsemble with a surpringly high accuracy 💌.

In the model section, we can see all the runs and the best model at the top.

![image](https://user-images.githubusercontent.com/19474037/148331554-71e061af-3d9a-476c-9cfe-be987f493227.png)
![image](https://user-images.githubusercontent.com/19474037/148332363-ad3ee42f-1a76-47ce-9453-97cdb7e893a7.png)
![image](https://user-images.githubusercontent.com/19474037/148332417-3deba64b-3a90-4196-bb0d-26d946cea6b4.png)


### Metrics

![image](https://user-images.githubusercontent.com/19474037/148332794-3d9ddafb-e215-4073-ae16-f5c296071f2e.png)

### Explanation

![image](https://user-images.githubusercontent.com/19474037/148332860-ba2eb798-3b84-4b96-91be-721790ede918.png)




## Example Readmes

To see how the specification has been applied, see the [example-readmes](example-readmes/).

## Related Efforts

- [Art of Readme](https://github.com/noffle/art-of-readme) - 💌 Learn the art of writing quality READMEs.
- [open-source-template](https://github.com/davidbgk/open-source-template/) - A README template to encourage open-source contributions.

## Maintainers

[@bhattajo](https://github.com/bhattajo).

## Contributing

Feel free to dive in! [Open an issue](https://github.com/RichardLitt/standard-readme/issues/new) or submit PRs.

Standard Readme follows the [Contributor Covenant](http://contributor-covenant.org/version/1/3/0/) Code of Conduct.





## References
<a id="1">[1]</a> 
**AciWebservice Class** Microsoft (2021). 
https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/deploy-to-cloud/model-register-and-deploy.ipynb
