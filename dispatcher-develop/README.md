# 5GENESIS Dispatcher

The 5GENESIS Dispatcher is the entry point to the system, offering the functionalities to an Experimenter through a single interface. These functionalites are know as the Open APIs, being able to interact with the key features of the underlying modules (as shown in the architecture diagram below) without actually exposing them
This implementation is based on a NGINX reverse proxy containerised in a Docker environment.
By default, The *Dispatcher* includes as added on modules, the [Validator](validator/README.md "Validator"), the [MANO Wrapper](mano/README.md "MANO Wrapper") and a Swagger environment to test the available features.

## Architecture

![Dispatcher architecture](./images/dispatcher_arch.png)

## Available features

The available features will depend on the features exposed by each dispatched module. The stable features are:

##### MANO

- Onboard VNFD
- List VNFDs
- Retrieve single VNFD
- Delete VNFD
- Onboard NSD
- List NSDs
- Retrieve single NSD
- Delete NSD

##### ELCM

- Launch experiment (create)
- Cancel execution
- Get execution logs

##### Validator

- Validation service: Validate Experiment descriptor, VNFD or NSD as a standalone service
- Validate and onboard directly the Experiment descriptor in the ELCM, and the VNF descriptor or the NS descriptor in the NFVO

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Pre-requisites

For running the 5Genesis Dispatcher, you will need:

- docker version >= 18.09.6
- docker-compose version >= 1.17.1
- Configuration files correctly filled up (including the config file inside the *validator* folder)
- NFVO + VIM

### Config file and installation

The Dispatcher needs to be configured properly before it is deployed. For that a simplified configuration file is offered: `dispatcher.conf`, which will have to be edited and adapted.
The file contains information of all the modules the Dispatcher forwards information to and how to do it. For each module, a new enabler will be added in the Dispatcher. It uses the following format:

    [module_name]
    PROTOCOL=[http|https]
    HOST=x.x.x.x -> IP or DNS name of the host component
    PORT=xxxx -> Port where the app API is available
    PATH=/ -> Base path of the application ("/" by default)

    [validator]
    PROTOCOL=http
    HOST=validator
    PORT=5100
    PATH=/

    [mano]
    PROTOCOL=http
    HOST=mano
    PORT=5001
    PATH=/

The config file template already includes the *validator* and the *mano* modules as they are included within the *Dispatcher*.
Once edited properly, the configuration should be applied and the containers built, based on the config file we have just created (*dispatcher.conf*):

`$ ./install.sh`

#### Example

    [elcm]
    PROTOCOL=http
    HOST=192.168.33.102
    PORT=4000
    PATH=/
    [validator]
    PROTOCOL=http
    HOST=192.168.33.105
    port=5100
    path=/api
    [mano]
    PROTOCOL=http
    HOST=192.168.33.105
    PROTOCOL=5001
    PATH=/

With the config file above, using the Validator API as an **example**, the dispatcher will translate the original request to a new one:
> Original URL: <http://192.168.33.105:5100/api/validate>

> Translated URL: <http://{dispatcher_ip}:{dispatcher:port}/validator/validate>

### Start

The start script will deploy and run the *Dispatcher* container, the [Validator](validator/README.md "Validator"), the [MANO Wrapper](mano/README.md "MANO Wrapper") and a Swagger environment to test the available features:

`$ ./start.sh`

Dispatcher will be accessible through port `8082`.

Swagger environment will be accessible through port `5002`.

### Stop

To stop the Dispatcher service just run the following:

`$ ./stop.sh`

## Try out the application

A swagger testing framework is deployed on port 5002 with the following API specifications:
![Dispatcher Swagger](./images/swagger.PNG)

## Logging

*Dispatcher* logs are available in the `log` folder of the application.

## Versioning

- 1.2.3 - Move to *fastjsonschema* for draft 7 schema validation
- 1.2.0 - *MANO wrapper* is included now inside the *Dispatcher* as a new container
- 1.1.0 - Added logging in the *Dispatcher* and the *Validator*
- 1.0.0 - First stable version including all features

## Next steps

- Add user authentication and registration
- Add cross-platform features
- Unify config files

## Authors

Javier Melian (javier.melian@atos.net)

## License

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   > <http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.