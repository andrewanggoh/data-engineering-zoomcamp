# Docker and Terraform

Ini dalah catatan saya untuk Modul ke-1 dari Data Engineering Zoomcamp yang mempelajari Docker, SQL, Terraform, dan Google Cloud Platform (GCP).

## Outline

- [1.1 Introduction to Docker](#11-Introduction-to-Docker)
- [1.2 Virtual Environment](#12-Virtual-Environment)
- [1.3 Dockerizing Pipeline](#13-Dockerizing-Pipeline)
- [1.4 Postgres Docker](#14-Postgres-Docker)
- [1.5 Data Ingestion](#15-Data-Ingestion)
- [1.6 Ingestion Script](#16-Ingestion-Script)
- [1.7 PGAdmin](#17-PGAdmin)
- [1.8 Dockerizing Ingestion](#18-Dockerizing-Ingestion)
- [1.9 Docker Compose](#19-Docker-Compose)
- [1.10 SQL Refresher](#110-SQL-Refresher)
- [1.11 Cleanup](#111-Cleanup)
- [1.12 Terraform Overview](#112-Terraform-Overview)
- [1.13 GCP Overview](#113-GCP-Overview)

## 1.1 Introduction to Docker

**Docker** adalah sebuah *containerization software* yang berguna untuk mengisolasi *software* mirip dengan *virtual machines* namun dengan cara yang lebih *lean*.

**Docker Image** adalah sebuah *snapshot* dari *container* yang bisa kita definisikan untuk bisa *run* di software kita, atau dalam hal ini *data pipeline*. Dengan *export* Docker images ke Cloud *providers* (AWS atau GCP), kita bisa *run* containers kita di sana.

### 1.1.1 Why Docker?

Docker menyediakan beberapa keunggulan:

- **Reproducibility**: Same environment everywhere
- **Isolation**: Applications run independently
- **Portability**: Run anywhere Docker is intalled

Docker digunakan dalam berbagai situasi:

- Integration tests: CI/CD pipelines
- Running pipelines on the cloud: AWS Batch, Kubernetes jobs
- Spark: Analytics engine for large-scale data processing
- Serverless: AWS Lambda, Google Functions

### 1.1.2 Basic Docker Commands

| Command                  | Description                                                               |
| ------------------------ | ------------------------------------------------------------------------- |
| `docker --version`       | Check Docker version                                                      |
| `docker run hello-world` | Run a simple container                                                    |
| `docker run <image>`     | Download (if there's not) and run container                               |
| `docker run -it`         | Run container interactively (get into container terminal)                 |
| `docker ps -a`           | List of all container (running or not)                                    |
| `docker rm <id>`         | Delete stopped container                                                  |
| `--rm`                   | Flag to automatically delete container after use                          |

### 1.1.3 Stateless Containers

> [!NOTE]
> Docker container itu *stateless* - perubahan apapun yang terjadi di dalam container TIDAK AKAN tersimpan jika container *killed* dan dijalankan kembali.
