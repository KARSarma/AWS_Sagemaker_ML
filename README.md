# Mobile Price Classification with AWS SageMaker 🚀

This repository demonstrates the power of **AWS SageMaker** in developing, deploying, and managing a machine learning solution for mobile price range prediction. By leveraging SageMaker's robust capabilities, this project illustrates an end-to-end workflow for training a **Random Forest Classifier** and deploying it for real-time predictions.

---

## Table of Contents

- [Project Overview](#project-overview)
- [AWS Services Used](#aws-services-used)
- [Dataset](#dataset)
- [Workflow](#workflow)
- [Setup Requirements](#setup-requirements)
- [Files and Scripts](#files-and-scripts)
- [Step-by-Step Instructions](#step-by-step-instructions)
- [Results](#results)
- [Future Directions](#future-directions)
- [License](#license)

---

## Project Overview

**Goal:** Predict the price range of mobile phones based on hardware specifications like battery power, RAM, and camera quality.

Using **AWS SageMaker**, this project integrates multiple tools and services to:
1. Train a machine learning model with scalable infrastructure.
2. Deploy the model as a RESTful endpoint for real-time predictions.
3. Monitor and manage the model lifecycle for continuous improvement.

---

## AWS Services Used

This project extensively utilizes the following AWS services to streamline the machine learning lifecycle:

### **1. Amazon SageMaker**
   - **Purpose**: The core service for training, deploying, and managing machine learning models.
   - **Features Used**:
     - **SKLearn Estimator**: For training the Random Forest Classifier in a managed environment.
     - **Model Deployment**: To host the trained model as a RESTful endpoint for real-time predictions.
     - **Endpoint Testing**: To test predictions using the deployed endpoint.
     - **CloudWatch Integration**: Automatically logs training and endpoint activity for monitoring and debugging.

### **2. Amazon S3 (Simple Storage Service)**
   - **Purpose**: Used as the primary storage for training and testing datasets, as well as model artifacts.
   - **Features Used**:
     - **Dataset Storage**: Uploaded `train-V-1.csv` and `test-V-1.csv` for SageMaker training.
     - **Model Artifact Storage**: Stored the trained model (`model.tar.gz`) after the SageMaker training job.
     - **Key Role**: Acts as a data source for SageMaker and a storage layer for output artifacts.

### **3. AWS IAM (Identity and Access Management)**
   - **Purpose**: Manages permissions and roles to securely access SageMaker, S3, and other AWS services.
   - **Features Used**:
     - **IAM Role for SageMaker**: Grants SageMaker permission to read/write from S3 and manage model endpoints.

### **4. Amazon CloudWatch**
   - **Purpose**: Monitors and logs SageMaker training and inference processes.
   - **Features Used**:
     - **Training Logs**: Tracks model training progress, resource utilization, and errors.
     - **Endpoint Logs**: Captures request/response details and errors during endpoint usage.

### **5. AWS SDK for Python (Boto3)**
   - **Purpose**: Python library used to interact with AWS services programmatically.
   - **Features Used**:
     - **S3 Operations**: Upload training and testing datasets.
     - **SageMaker Operations**: Create training jobs, deploy models, and invoke endpoints.

### **6. Amazon EC2 (Elastic Compute Cloud)**
   - **Purpose**: Underlying compute resource for running SageMaker training and endpoint instances.
   - **Instance Types Used**:
     - **ml.m5.large**: For training the Random Forest model.
     - **ml.m4.xlarge**: For deploying the model as a RESTful endpoint.

### **7. SageMaker Containers**
   - **Purpose**: Pre-built Docker images provided by SageMaker for running specific frameworks like scikit-learn.
   - **Features Used**:
     - **Scikit-learn Container**: Used the `0.23-1` version to execute the training script.

---

## Dataset

The dataset contains 2,000 samples with the following characteristics:
- **Features:** `battery_power`, `ram`, `fc`, `int_memory`, `px_height`, etc.
- **Label:** `price_range` (0 to 3, representing increasing price tiers).

### Example:
| battery_power | ram  | fc | int_memory | price_range |
|---------------|------|----|------------|-------------|
| 1450          | 794  | 4  | 31         | 1           |
| 1218          | 1667 | 14 | 39         | 1           |

---

## Workflow

### **1. Data Preparation**
   - The dataset is split into **training (85%)** and **testing (15%)** subsets.
   - Training and testing data are uploaded to **Amazon S3** under:
     ```
     s3://mobsagemaker213/sagemaker/mobile_price_classification/sklearncontainer/
     ```

### **2. Model Training**
   - The `script.py` defines the model training process using SageMaker’s **SKLearn Estimator**.
   - The training job runs on an **ml.m5.large** instance with the following configuration:
     - **Hyperparameters:** `n_estimators=100`, `random_state=0`
     - **Output:** Model artifact saved as `model.joblib`.

### **3. Model Deployment**
   - The trained model is deployed as a **SageMaker endpoint** using the `SKLearnModel` API.
   - Endpoint instance type: **ml.m4.xlarge**.

### **4. Real-Time Inference**
   - The endpoint is tested with unseen test data to ensure accuracy.
   - Results are compared against the ground truth.

---

## Setup Requirements

1. **AWS Account** with SageMaker enabled.
2. **IAM Role** with permissions for SageMaker, S3, and CloudWatch.
3. **Python Environment**:
   - Install `boto3` and `sagemaker` libraries:
     ```bash
     pip install boto3 sagemaker
     ```

---

The **AWS Services Used** section now prominently showcases all the tools and technologies leveraged from AWS, underlining your expertise in using SageMaker and its ecosystem effectively. Let me know if you'd like to further customize!
