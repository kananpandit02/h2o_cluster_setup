# 🚀 Distributed Machine Learning Using H2O.ai Cluster Setup

> A hands-on project demonstrating how to build a distributed machine learning cluster using H2O.ai across multiple nodes.

---

## 🧭 Overview

This project focuses on deploying a **multinode H2O.ai cluster** for distributed machine learning. The cluster is used to train and evaluate models on real-world datasets efficiently. We showcase the entire setup from scratch: environment creation, H2O cluster formation, and executing a Random Forest classifier using Python.

---

## 🎯 Objective

- To build and configure a distributed H2O.ai cluster
- To leverage cluster-based computation for machine learning tasks
- To train and evaluate models (Iris dataset) using `H2ORandomForestEstimator`
- To provide a reusable, open, and academic framework for similar projects

---

## 💡 Motivation

With the increasing need for scalable machine learning, traditional single-node setups often fall short. H2O.ai offers a powerful, open-source framework that supports multinode computation out-of-the-box. This project aims to make the setup and usage of an H2O.ai distributed cluster accessible and easy to reproduce for students, researchers, and educators.

---
## 📍 Course Project | Ramakrishna Mission Vivekananda Educational and Research Institute, Belur Math  
🧠 Team: Tom and Jerry  
👨‍💻 Members: Kanan Pandit , Sudam Paul  
📅 Date: April 26, 2025



## 📁 Project Structure
## 📦 H2O_Cluster_Project


- [`sample_prog.py`](./sample_prog.py) – ML model script using H2O  
- [`h2o_flatfile.txt`](./h2o_flatfile.txt) – IP list of nodes for H2O cluster  
- [`Steps to Create H2O Cluster.txt`](./Steps%20to%20Create%20H2O%20Cluster.txt) – Step-by-step setup guide  
- [`DML_H2O.pdf`](./DML_H2O.pdf) – Final academic project report  
- [`README.md`](./README.md) – Project documentation  
 ## ⚙️ Step-by-Step: How to Create a Multinode H2O.ai Cluster

This section walks you through setting up a multinode H2O cluster using virtual environments, Java, and H2O's flatfile configuration method.

---

### ✅ Requirements

- **Java**: Version 8 to 17  
- **Python**: Version 3.12  
- **H2O**: Latest release  
- **Other Tools**: Virtual environment, wget, unzip  
- **Python Libraries**: `h2o`, `numpy`, `pandas` (optional)

---

### 🛠️ 1. Create and Activate a Virtual Environment

```bash
python3 -m venv env
source env/bin/activate
```
### 🔄 2. System Update and Java Installation
```bash
sudo apt update
sudo apt install openjdk-11-jdk
```
H2O requires Java 8 or later. Use java -version and javac -version to verify.
### 📦 3. Install H2O Python Package
```bash
pip install h2o
```
### 💾 4. Download and Extract the H2O Multinode Binary
```bash
wget https://h2o-release.s3.amazonaws.com/h2o/3.46.0.7 -O h2o-latest.zip
unzip h2o-latest.zip
```
Alternatively, search for H2O 3.46.0.7 manually in a browser and download from the official site.
### 🗃️ 5. Move h2o.jar to Virtual Environment Directory
```bash
cp h2o-3.46.0.7/h2o.jar .
```
### 🧾 6. Create a Flatfile with Cluster Node IPs
Create a file called h2o_flatfile.txt in your environment directory with the following format:
```bash
172.20.252.58
172.20.252.53
```
Each line must contain a node's IP address in the cluster.
### 🚀 7. Start the H2O Cluster on Each Node
# On Node 1 (Master):
```bash
java -Xmx2g -jar h2o.jar -name trial1 -port 54323 \-flatfile h2o_flatfile.txt -network 172.20.252.0/24
```
# On Node 2 (Worker):
```bash
java -Xmx2g -jar h2o.jar -name trial1 -port 54323 \-flatfile h2o_flatfile.txt -network 172.20.252.0/24 \-peer 172.20.252.53:54323
(Replace 172.20.252.53 with your actual master node IP.)
```
(Replace 172.20.252.53 with your actual master node IP.)

## 📌 Explanation of the -network Option
The -network 172.20.252.0/24 argument:

Uses CIDR notation for subnetting.

Tells H2O to discover and communicate with nodes in the range 172.20.252.1 to 172.20.252.254.

Helps automatic node discovery within the same LAN/subnet.

Subnet Info	Value
Subnet Mask	255.255.255.0
Range	172.20.252.1–254
Broadcast	172.20.252.255
### 🧪 8. Run the ML Program
Open a new terminal in the same environment and run your Python program:
```bash
python sample_prog.py
```
### 🛑 9. Shutdown the H2O Cluster
The cluster will automatically shut down if coded like this:
```bash
h2o.cluster().shutdown()
```
Or manually: use CTRL + C on each Java terminal session.
### ✅ Final Notes
All nodes must be in the same network/subnet

Use the same h2o_flatfile.txt and h2o.jar on all nodes

Ensure ports (e.g. 54323) are open across firewalls

Nodes should be able to ping each other

## 📊 Results

The Distributed Random Forest (DRF) model was evaluated on the Iris dataset using H2O.ai in a distributed cluster setup. The model was trained on 80% of the dataset and tested on the remaining 20% (27 samples).

---

### 🔍 Model Performance Metrics

| Metric                       | Value                                  |
|------------------------------|----------------------------------------|
| **Accuracy**                 | 92.59%                                 |
| **MSE (Mean Squared Error)** | 0.0378                                 |
| **RMSE (Root Mean Squared Error)** | 0.1945                         |
| **LogLoss**                  | 0.1049                                 |
| **Mean Per-Class Error**     | 0.1444                                 |
| **Overall Error Rate**       | 7.41% (2 misclassifications out of 27) |
| **AUC / AUCPR**              | Not computed (due to multiclass setup or auc_type=NONE) |

---

### 📊 Confusion Matrix

| Actual \ Predicted | Setosa | Versicolor | Virginica | Error | Count |
|--------------------|--------|------------|-----------|--------|--------|
| **Setosa**         | 14     | 0          | 0         | 0.00   | 14     |
| **Versicolor**     | 0      | 9          | 1         | 0.10   | 10     |
| **Virginica**      | 0      | 1          | 2         | 0.33   | 3      |
| **Totals**         | 14     | 10         | 3         | —      | 27     |

> 📉 Misclassifications:
> - 1 **Versicolor** → misclassified as **Virginica**  
> - 1 **Virginica** → misclassified as **Versicolor**

---

### 🎯 Classification Report

| Class         | Precision | Recall | F1-Score |
|---------------|-----------|--------|----------|
| **Setosa**     | 1.00      | 1.00   | 1.00     |
| **Versicolor** | 0.90      | 0.90   | 0.90     |
| **Virginica**  | 0.67      | 0.67   | 0.67     |
| **Macro Avg**  | **0.86**  | **0.86** | **0.86** |

---

### 🧠 Interpretation

- **Setosa** is perfectly predicted, as expected due to its well-separated features.
- Minor confusion exists between **Versicolor** and **Virginica**, which is common in this dataset.
- Despite a small test set (only 3 Virginica samples), the model maintains a strong **overall accuracy of 92.59%**.
- The F1-Score for **Virginica** is lower due to the limited sample size and 1 misclassification.

✅ These results demonstrate high performance using H2O’s distributed Random Forest, even with class imbalance in the test set.



## 🔁 Optional: Use AutoML with Your Own Dataset

If you're interested in extending this project further or applying it to your own dataset, you can easily integrate H2O’s AutoML functionality. This will automatically train and evaluate multiple machine learning models.

### 🧪 Steps to Use AutoML

```python
import time
from h2o.automl import H2OAutoML

# ⏰ Start time
localtime = time.asctime(time.localtime(time.time()))
print("Local current time:", localtime)

# ⚙️ Run AutoML
aml = H2OAutoML(
    max_models=15,
    seed=1234,
    exclude_algos=["StackedEnsemble"],  # Optional: exclude ensemble if needed
    balance_classes=True
)

aml.train(
    x=featureColumns,
    y=targetColumn,
    training_frame=train,
    validation_frame=valid
)

# ⏰ End time
localtime = time.asctime(time.localtime(time.time()))
print("Local current time:", localtime)
```







### 🙏 Acknowledgements

Special thanks to:

**Champak Kumar Dutta**  
Assistant Professor, Department of Data Science  
RKMVERI, Belur Math, West Bengal  

For his guidance, mentorship, and continuous encouragement.

## 📜 License

This project is licensed under the **Academic Use Only License**.

- ✅ You are free to use, modify, and distribute this work **for academic, research, and educational purposes only**.
- ❌ Commercial use, redistribution, or integration into proprietary products is **strictly prohibited** without prior written permission.

If you wish to use this project beyond academic contexts, please contact the authors for licensing terms.


## 🌐 Connect With Us

**Kanan Pandit (B2430051)**  
🌐 [Portfolio](https://kananpanditportfolio.netlify.app/)  
✉️ kananpandit02@gmail.com  

**Sudam Paul (B2430023)**  
🌐 [Portfolio](https://sudam23.github.io/My_Portfolio/)  
✉️ 2002sudam@gmail.com  

**Institution**  
Ramakrishna Mission Vivekananda Educational and Research Institute  
📍 Belur Math, Howrah, West Bengal  


