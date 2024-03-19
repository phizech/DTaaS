# Digital Twin Cockpit Manager

## Setup
The [dtcms.zip](./dtcms.zip) file comprises setup scripts for preconfigured components for MongoDB, the BasyxInfrastructure, and the DTCM itself. These components are designed to operate independently across various systems and are not required to be deployed on the same server. Nonetheless, for optimal performance, it is advisable to minimize the distance between components topologically to decrease latency.

To setup one or more components please extract the [dtcms.zip](./dtcms.zip) and navigate to the setup folder.


```bash
sudo apt-get install -y unzip
```

```bash
unzip ./dtcms.zip
cd setup
```

### Setup Docker
Each component is engineered to operate as a Docker container, facilitating portability across various cloud providers. Thus, Docker must be installed and operational to execute the setup scripts for each component.

Additionally, a setup script is provided to install Docker and initiate a Docker daemon. This script can be executed using the following command. If Docker is already installed and running, this step can be omitted.

```bash
sudo bash install_docker.sh
```

---

### Setting the Environment

**Before starting the individual components, ensure that all environment variables in the .env file are set!**

You can set the relevant environment variables by opening the file using:

```bash
nano .env
```

Be aware that the variables MONGODB_HOST and BASYX_HOST must not be set to localhost/127.0.0.1 or any other IP address that interferes with the Docker network. \
If you intend to install the components locally or on the same system, please use the IP address of the machine in the local network (e.g., 192.168.0.42).

---

### Setup MongoDB
To set up MongoDB as a Docker container use:

```bash
sudo bash ./mongodb/setup.sh
```

### Setup BasyxInfrastructure
To set up Basyx and an MQTT Broker as a Docker container:\
**Please ensure that MongoDB is already running when starting the Basyx Infrastructure.**

```bash
sudo bash ./basyxinfrastructure/setup.sh
```

This will start an Eclipse Mosquitto MQTT Broker with the credentials basyxmqtt_user and basyxmqtt_pass defined in the .env file. \
Additionally, the registry server and AAS server from Eclipse Basyx will be started and connections to the MongoDB and the MQTT Broker are being established.

The individual components can be accessed over the following endpoints:

| Component             | Endpoint                                                                   |
| --------------------- | -------------------------------------------------------------------------- |
| MQTT                  | [tcp://{BASYX_HOST}:1884](tcp://{BASYX_HOST}:1884)                         |
| Basyx Registry Server | [http://{BASYX_HOST}:14000/registry](http://{BASYX_HOST}:14000/registry)   |
| Basyx AAS Server      | [http://{BASYX_HOST}:14001/aasServer](http://{BASYX_HOST}:14001/aasServer) |

### Setup DTCM
To set up Digital Twin Cockpit Manager as a Docker container:\
**Please ensure that MongoDB is already running when starting the DTCM.**

```bash
sudo bash ./digitaltwincockpitmanager/setup.sh
```

After starting the DTCM the Application can be accessed over [{DTCM_HOST}:8443](https://{DTCM_HOST}:8443)