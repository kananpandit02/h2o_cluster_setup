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




