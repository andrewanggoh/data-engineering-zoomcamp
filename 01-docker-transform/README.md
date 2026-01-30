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

# 1.1 Introduction to Docker

**Docker** adalah sebuah *containerization software* yang berguna untuk mengisolasi *software* mirip dengan *virtual machines* namun dengan cara yang lebih *lean*.

**Docker Image** adalah sebuah *snapshot* dari *container* yang bisa kita definisikan untuk bisa *run* di software kita, atau dalam hal ini *data pipeline*. Dengan *export* Docker images ke Cloud *providers* (AWS atau GCP), kita bisa *run* containers kita di sana. Apapun yang kita lakukan di Docker itu terisolasi dari host machine.

## 1.1.1 Why Docker?

Docker menyediakan beberapa keunggulan:

- **Reproducibility**: Same environment everywhere
- **Isolation**: Applications run independently
- **Portability**: Run anywhere Docker is intalled

Docker digunakan dalam berbagai situasi:

- Integration tests: CI/CD pipelines
- Running pipelines on the cloud: AWS Batch, Kubernetes jobs
- Spark: Analytics engine for large-scale data processing
- Serverless: AWS Lambda, Google Functions

> [!NOTE]
> - **CI** means *Contionuous Integration* dan **CD** means *Continuous Delivery/Deployment* untuk make sure kode konsisten dan minim bug.
> - Serverless means model cloud computing untuk buat aplikasi tanpa perlu mengelola, menyediakan, atau memelihara server fisik/virtual.

## 1.1.2 Basic Docker Commands

| Command                  | Description                                                               |
| ------------------------ | ------------------------------------------------------------------------- |
| `docker`                 | Check Docker installed or not                                             |
| `docker --version`       | Check Docker version                                                      |
| `apt update`             | Update libraries on repo                                                  |
| `apt install <library>`  | Install library. Example: `apt install python3`                           |
| `docker run hello-world` | Run a simple container                                                    |
| `docker run <image>`     | Download (if there's not) and run container and back to our computer      |
| `docker run -it`         | Run container interactively (get into/inside the container terminal)      |
| `docker ps -a`           | List of all container (running or not)                                    |
| `docker rm <id>`         | Delete stopped container                                                  |
| `--rm`                   | Flag to automatically delete container after use                          |

> [!NOTE]
> Basic commands in Ubuntu:
> - `ls`: list of files in the folder
> - `cd <nama folder>`: get into the folder
> - `cd ..`: get out from the folder
> - `pwd`: check current path of working directory

## 1.1.3 Stateless Containers

> [!NOTE]
> Docker container itu *stateless* (doesn't preserve state) - perubahan apapun yang terjadi di dalam container **TIDAK AKAN** tersimpan jika kita keluar container dan dijalankan kembali.

Docker image dapat dibuat dengan menggunakan code sebagai berikut:
```
docker run -it ubuntu

# Check python
python -V
```

Hasilnya akan terlihat bahwa tidak ada python yang ter-install meskipun di host machine ter-install python. Ini bukti kalau docker machine tidak sama dengan host machine. 

## 1.1.4 Different Base Images

Pada bagian ini, kita akan menggunakan docker image dengan `python:3.13.11-slim` sebagai base images rather than Docker Image `ubuntu`. Append `-slim` digunakan untuk menggunakan versi yang lebih ringan. Ketika `run` docker image tersebut kita akan masuk ke Python session.

Agar dapat masuk ke `bash` session dimana kita bisa berinteraksi dengan OS Linux/Unix, kita dapat menambahkan `--entrypoint` seperti berikut:
```
docker run -it --entrypoint=bash python:3.13.11-slim
```

Kemudian, kita juga bisa buat sebuah file (tapi ini *antipattern*) untuk membuktikan kondisi *stateless*:
```
# create file yang isinya '123'
echo 123 > file 

# liat isi file bernama `file`
cat file
```

Ketika kita exit (`Ctrl+D`) dan masuk lagi, file tersebut hilang.

## 1.1.5 Managing Containers

Namun, sebenarnya **hal itu tidak sepenuhnya benar**. Kondisi atau *state* tersebut tersimpan somewhere. Kita bisa liat stopped containers:
```
docker ps -a
```

Melihat id tiap containers yang ada
```
docker ps -aq
```

Menghapus seluruh docker containers yang ada
```
docker rm `docker ps -aq`
```

Jadi, **best practice** dalam menjalankan docker adalah dengan menambahkan `--rm` agar container tidak tersimpan dan take memory space
```
docker run -it --rm python:3.13.11
# add -slim to get a smaller version
```

## 1.1.6 Volumes

Jadi, sekarang kita tau dengan docker kita bisa restore container manapun ke initial state dengan reproducible manner. Bagaimana dengan datanya? Bagaimana cara kita *preserve state*? Cara umumnya adalah dengan ***volumes***.

Misal kita buat sebuah directory `test` dan berisi files didalamnya
```
mkdir test
cd test
touch file1.txt file2.txt file3.txt
echo "Hello from host" > file1.txt
cd ..
```

Lalu, kita buat simple script `test/list_files.py` yang menunjukkan files dalam folder:
```
from pathlib import Path

current_dir = Path.cwd()
current_file = Path(__file__).name

print(f"Files in {current_dir}:")

for filepath in current_dir.iterdir():
    if filepath.name == current_file:
        continue

    print(f"  - {filepath.name}")

    if filepath.is_file():
        content = filepath.read_text(encoding='utf-8')
        print(f"    Content: {content}")
```

Kita bisa langsung menjalankannya dari host machine dengan `python script.py`. Kita bisa *execute* dari dalam docker container dengan menggunakan ***volumes***. Volumes memungkinkan folder `test` available di host machine sekaligus di dalam docker container. Kita dapat menggunakan fitur ini dengan parameter `-v <path_on_host>:<map_location_on_docker>`
```
docker run -it \
    --rm \
    -v $(pwd)/test:/app/test \
    --entrypoint=bash \
    python:3.9.16-slim
```

Setelah masuk ke docker containernya, kita bisa liat kalau ada folder `app` yang di dalamnya ada folder `test` berisi files yang sama dengan apa yang ada di host machine. Files from the host machine are accessible in the container!

# 1.2 Virtual Machine
