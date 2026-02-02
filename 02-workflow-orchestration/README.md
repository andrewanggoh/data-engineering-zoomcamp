# Workflow Orchestration

Ini dalah catatan saya untuk Modul ke-2 dari Data Engineering Zoomcamp yang mempelajari workflow orchestration menggunakan Kestra sebagai *orchestrator*.

## Outline

- [2.1 Introduction to Workflow Orchestration](#21-introduction-to-workflow-orchestration)
- [2.2 Getting Started with Kestra](#22-getting-started-with-kestra)
- [2.3 Hands-On Coding Project: Build ETL Data Pipelines with Kestra](#23-hands-on-coding-project-build-etl-data-pipelines-with-kestra)
- [2.4 ELT Pipelines in Kestra: Google Cloud Platform](#24-elt-pipelines-in-Kestra-Google-Cloud-Platform)
- [2.5 Using AI for Data Engineering in Kestra](#25-Using-AI-for-Data-Engineering-in-Kestra)

## 2.1 Introduction to Workflow Orchestration

Bagian ini berisi tentang dasar dari *workflow orchestration*, kegunaannya, dan bagaimana Kestra cocok sebagai *orchestrator* dalam *workflow orchestration*.

### 2.1.1 - What is Workflow Orchestration?

Workflow orchestration adalah sebuah sistem yang memanfaatkan berbagai tools (code, cloud, platform, dll.) untuk menjalankan sebuah *workflow*. Sebagaimana orkestrasi musik yang memiliki *conductor*, *workflow orchestration* memiliki *orchestrator* yang berperan untuk mengatur alur kerja tiap tools. Pada camp ini, kita akan menggunakan Kestra sebagai *orchestrator*.

### 2.1.2 - What is Kestra?

Kestra adalah sebuah platform *open-source*, *infinitely scalable*, dan *event-driven orchestration* yang menyederhanakan pembuatan *workflows* yang *scheduled* dan *event-driven*. Kestra memungkinkan kita untuk membangun sebuah *reliable workflow* dengan hanya beberapa baris YAML dengan mengadopsi infrastruktur sebagai *code practices* untuk *data and process orchestration*. Berikut fitur dari Kestra:

- **Building workflow with code, no code, and AI**: bahkan ketiganya dapat digunakan secara bergantian.
- **Lot of plug-ins**: untuk menghubungkan dengan berbagai tools, seperti Postgre, GCP, Python, dll.
- **Full visibility into workflows**: ada *detailed logging* untuk monitor workflow dan notify apakah ada error atau tidak.
- **Scheduled and event-based triggers**: for automatically start the workflow (when data arrives in data lake, nightly job running overnight, or whenever we decide).

## 2.2 Getting Started with Kestra

Bagian ini berisi tentang cara install Kestra, konsep penting dalam pembuatan *workflow orchestration* di Kestra, dan eksekusi script Python di Kestra.

### 2.2.1 Installing Kestra

Pada bagian ini kita akan menggunakan Postgre Database dan PGAdmin dalam set up docker compose. Selain itu, kita juga akan menggunakan PG Database tambahan untuk Kestra dapat menyimpan data dan workflow dan metadata Kestra. Kita juga akan menambahkan 2 *volumes* baru, yaitu

1. kestra_postgres_data
2. kestra_data

Kita juga akan menggunakan container kestra pada docker compose. Full code script-nya: `docker-compose.yaml` di repo ini. Kemudian, *run* docker-nya:
```
docker compose up
```

### 2.2.2 Learn the Kestra Concepts



## 2.3 Hands-On Coding Project: Build ETL Data Pipelines with Kestra

## 2.4 ELT Pipelines in Kestra: Google Cloud Platform

## 2.5 Using AI for Data Engineering in Kestra
