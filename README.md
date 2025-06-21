# 🚀 Distributed Machine Learning Using H2O.ai Cluster Setup

> A practical implementation of setting up a multinode H2O cluster for distributed machine learning. This guide walks through environment setup, flatfile-based clustering, and training a Random Forest model on the Iris dataset.

---

## 📁 Project Structure
## 📦 H2O_Cluster_Project

- [`sample_prog.py`](./sample_prog.py) – ML model script using H2O  
- [`h2o_flatfile.txt`](./h2o_flatfile.txt) – IP list of nodes for H2O cluster  
- [`Steps to Create H2O Cluster.txt`](./Steps%20to%20Create%20H2O%20Cluster.txt) – Step-by-step setup guide  
- [`README.md`](./README.md) – Project documentation
-
-
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



