# 🚀 Spark & Hadoop Cluster with Docker Compose

This project sets up an Apache Hadoop and Apache Spark environment using Docker Compose. It's ideal for local development, testing distributed data processing, and running Spark jobs on YARN.

---

## 🧱 Stack Components

| Service         | Image                                               | Ports       | Role                            |
|-----------------|------------------------------------------------------|-------------|----------------------------------|
| **NameNode**    | `bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8`    | `9870`, `9000` | HDFS master node                 |
| **DataNode**    | `bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8`    |             | HDFS worker node                |
| **ResourceManager** | `bde2020/hadoop-resourcemanager:2.0.0-hadoop3.2.1-java8` | `8088`      | YARN job scheduler               |
| **NodeManager** | `bde2020/hadoop-nodemanager:2.0.0-hadoop3.2.1-java8` |             | YARN worker node                |
| **Spark Client**| `bde2020/spark-master:3.3.0-hadoop3.3`               | `4040`, `7077`, `8090` | Spark CLI and client for submitting jobs |
<!-- | Spark History Server | Optional | `18080` | Job log visualization (if enabled) | -->

---

## 📂 Folder Structure

```bash
.
├── docker-compose.yml # Docker Compose cluster definition
├── hadoop.env # Shared environment variables
├── README.md # Project documentation

├── conf/ # Hadoop config files
│ ├── core-site.xml
│ ├── mapred-site.xml
│ └── yarn-site.xml

├── apps/ # Spark job scripts (Python, Scala, etc.)
│ └── example_job.py # Example PySpark job script

├── data_io/ # Input/output data for Spark jobs
│ ├── input/ # Input CSV/JSON/etc.
│ └── output/ # Output files from Spark jobs

```
---


## 🚀 Getting Started

### 1. Clone the Repository

```bash
.
git clone https://github.com/yourusername/hadoop-spark-docker.git
cd hadoop-spark-docker
```

### 2. Start the Cluster
```bash
docker-compose up -d
✅ Wait 30–60 seconds for services to fully initialize.

```

🖥️ Web Interfaces  
| UI                       | URL                     |
|--------------------------|-------------------------|
|NameNode Web UI           |	http://localhost:9870  |
|YARN ResourceManager UI   |	http://localhost:8088  |
|Spark Application UI      |	http://localhost:4040  |
|Spark Standalone Master UI|	http://localhost:8090  |

🧪 Run a Spark Job on YARN
Enter the Spark Client container

```bash
docker exec -it spark-client /bin/bash
Submit a Spark job to YARN

```

```bash
spark-submit \
  --master yarn \
  --deploy-mode client \
  /opt/spark-apps/example_job.py
Replace wordcount.py with your actual application script in /apps.
```

🔧 Configuration
Place your custom Hadoop configuration files in the conf/ directory:

core-site.xml

mapred-site.xml

These are mounted into Spark and Hadoop containers for proper integration.

Environment variables are stored in hadoop.env for easier management and reuse.

🛑 Stopping the Cluster
```bash
docker-compose down
To remove all containers, networks, and volumes (⚠️ destructive):
```

```bash
docker system prune -a --volumes
```
📌 Notes
This setup is designed for local testing and learning, not production.

You can scale DataNodes and NodeManagers by duplicating their service blocks in docker-compose.yml and updating names/ports.

Spark can run in YARN mode (preferred here) or standalone (with minimal config changes).