---
title: "GCP-InfluxDB"
date: 2020-11-23
tags: [InfluxDB, GCP(Google Cloud Platform), Python, project ewaso, DeKUT-DSAIL]
header:
    image: "assets/img/dat2.png"
excerpt: "Setting up a Google Cloud Platform - InfluxDB Virtual Machine Instance to Collect Time Series Data"
mathjax: true
classes: wide
---

## Introduction.
The following are brief steps on how to setup / launch an InfluxDB Virtual Machine on GCP (Google Cloud Platform), install python 3.7 on it and run a python script that queries the data from an IOT Network server like TTN (The Things Network).

### Pre-Requisites / Requirements.
- A Google Cloud Platform (GCP) account with access to the GCP Marketplace and Credit.
- Access to GCP Cloud Shell or the Gcloud SDK and command line tools.
### Create a project
Create a project on GCP - where you will deploy the InfluxDB Virtual Machine instance 
### Deploying the InfluxDB Virtual Machine Instance ( Full Package provide by Google )
To deploy an InfluxDB instance log into your Google Cloud Platform account and navigate to InfluxDB in the GCP marketplace using the links provided:
`GCP CONSOLE` - `COMPUTE ENGINE` -  `GCP MARKETPLACE` - `DATABASES` - `InfluxDB Virtual Machine (VM)`

{% include figure image_path="assets/img/inf1.PNG" alt="*Fig 1: Launch Page*" caption="*Fig 1: Launch Page*" %}

After clicking on the launch button, you wil be prompted to customize the InfluxDB virtual machine to be launched.(Customization can be done or you can lauch it as it is)

{% include figure image_path="assets/img/inf2.PNG" alt="*Fig 2: Deployment Page*" caption="*Fig 2 : Deployment Page*" %}
{% include figure image_path="assets/img/inf3.PNG" alt="*Fig 3: Deployment Page*" caption="*Fig 3 : Deployment Page*" %}

Name the instance and click deploy. Copy the instance password, username and IP address provide to a text document for storage.

{% include figure image_path="assets/img/inf4.PNG" alt="*Fig 4: Deployment Page*" caption="*Fig 4 : Deployment Page*" %}

After deployment, click on GCP external address and change the status of the InfluxDB VM instance to static. After setting up the static IP address, you can access the VM instance using the SSH protocal. (The SSH protocol (also referred to as Secure Shell) is a method for secure remote login from one computer to another. It provides several alternative options for strong authentication, and it protects the communications security and integrity with strong encryption.)

{% include figure image_path="assets/img/vm1.PNG" alt="*Fig 5: Deployment Page*" caption="*Fig 5 : Deployment Page*" %}

## Python 3.7 Installation.
The next step after accessing the instance is to install python (to enable querying of data from an IoT network server)in the said instance. To make this possible python 3.7 with pip is needed and its installation can be achieved by running the following commands consecutively:

- `sudo apt update`
- `sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev curl`
- `mkdir /tmp/Python37`
- `cd /tmp/Python37`
- `curl -O https://www.python.org/ftp/python/3.7.3/Python-3.7.3.tar.xz`
- `tar -xf Python-3.7.3.tar.xz`
- `cd /tmp/Python37/Python-3.7.3`
- `./configure --enable-optimizations`
- `nproc` to know the number of cores) in my case servers core
- hence `make -j 1 ` (2- represents the number of cores)
- `sudo make altinstall ` (installation after building the binaries)
- `python3.7 --version` (verifying your installation)

## Virtual Environment Creation
If the querying-mqtt python script is located on Github you can follow the following commmands. An example of a TTN (The Things network) - InfluxDB querying script can be accessed at [Ciira WA Maina Github - ttn_to_db.py](https://github.com/ciiram/mdot-maji/blob/master/ttn_to_db.py). The requirements to run the script [Ciira WA Maina Github - requirements.txt](https://github.com/ciiram/mdot-maji/blob/master/requirements.txt). 

- Access the python folder path: `cd /tmp/Python37/Python-3.7.3`
- git installation: `sudo apt-get install git`
- Cloning a repo with the `Querying script` and `requirements.txt`: `git clone https://github.com/username/gcp-scripts.git`(example)
- Access the cloned repo : `cd gcp-scripts/`

If the querying script and requirement.txt are on your localhost you can create duplicates on the Virtual Machine and then copy the content from your localhost and paste it.**Procedure** (`touch ttn_to_db.py` , `nano ttn_to_db.py` , copy the code from the python file in your local host and paste them on to the terminal. , `press (cntrl+O) to save`, then enter and lastly `press (cntrl+x)` to exit the terminal) and (`touch requirements.txt`, `touch requirements.txt`, copy the requirements.txt from the text file in your local host and paste them on to the terminal, `press (cntrl+O)` to save, then enter and lastly press `(cntrl+x)` to exit the terminal):  (**NB** THE SAME COMMANDS CAN BE USED TO EDIT THE SCRIPTS = `nano ttn_to_db.py` TO OPEN )

- **Virtual environment creation**: `python3.7 -m venv venv-query`
- Activation of the virtual environment: `source venv-query/bin/activate`
- Installation of the requirements: `pip install -r requirements.txt` (requirements.txt file)
## Querying and database creation
- Before executing the querying script below are changes to be made:
```python
    app_id = "TTN / network server application id"
    access_key = "TTN / application access key "
```
- Key in the InfluxDB virtual machine credentials as shown in the code snippet below
```python
    db_client = InfluxDBClient(host='instance external IP address', port=8086, username='instance username', password='instance password', ssl=True)
    db_client.create_database('database name')
    db_client.switch_database('database name')

    def uplink_callback(msg, client):
```
- Key in the gateway identification (In this case a registered LoRa gateway)
```python
    GTW_IDS = ['gateway1', 'gateway2', 'gateway3', 'gateway4'] # gateway of interest
```
- After making the changes the script should be ready for execution using Nohup. Nohup , short form for "**No Hung Up**", is a command in Linux systems thata keep processes running even after exiting the Shell or terminal. In this case, Nohup will enable the script to continuously query the data from TTN (automation),hence, the moment a device sends sensor data to the TTN it is recorded in the Influx database.
command : `nohup python ttn_to_db.py`
## Database Access 
- Database access: `influx -ssl -unsafeSsl -username 'instance username' -password 'instance password'`
- `show databases`
- `use database name`
- `show measurements`
- `select * from "measurement on record"`

## Acknowledgement
Dr Ciira Wa Maina - Director, Centre for Data Science abnd Artificial Intelligence (DSAIL) - Dedan Kimathi University of Technology

