# 5GENESIS Validator

This module validates the correct syntax of the Experiment descriptor and the descriptors of the Network Service (VNFD + NSD). It allows two options: a stand alone validation service that returns whether the descriptor is correct or not and the onboarding option, which, besides the validation, performs the onboard of the descriptor after a successful validation.

**Note**: the Experiment descriptor validation is based on the latest version of the Experiment descriptor information model defined below. Therefore, older versions are not supported.

## Experiment descriptor definition

![Experiment descriptor](./images/ED.png)

All fields are required except the *Reservation_time* one, that is only required if the experiment is attended.

## Experiment descriptor example

    {
        "Id": 123,
        "Name":"sfafd",
        "User": 123,
        "Platform": "malaga",
        "TestCases": ["TC1", "TC2"], 
        "UEs": ["UE1", "UE2"], 
        "Slice": "slice id", 
        "NS": "ns_id",
        "NS_Placement": "Edge",
        "Limited": true,
        "Type_experiment": false,
        "Scenario": ["scenario1", "scemarop2"], 
        "Application": ["app1"],
        "Attended": true, 
        "Reservation_time": 123 
    }    

## Network service descriptors

The 5GENESIS Validator also validates VNFD and NSD packages.
For now only OSM descriptors are supported:

[VNFD information model](http://osm-download.etsi.org/ftp/osm-doc/vnfd.html "VNFD IM")

[NSD information model](http://osm-download.etsi.org/ftp/osm-doc/nsd.html "NSD IM")

## Available functions

The Validator is available through the Dispatcher port (8082) and using the endpoint `/validator` with POST methods:

- Validate Experiment descriptor:
`/validate/ed`

Example:
> curl -X POST -d @descriptor.json <http://{dispatcher_host}:8082/validator/validate/ed>

- Validate and onboard Experiment descriptor:
`/onboard/ed`

Example:
> curl -X POST -d @descriptor.json <http://{dispatcher_host}:8082/validator/onboard/ed>

- Validate VNF descriptor:
`/validate/vnfd`

Example:
> curl -X POST -F "vnfd=@./vnf_descriptor.tar.gz" <http://{dispatcher_host}:8082/validator/validate/vnfd>

- Validate and onboard VNF descriptor:
`/onboard/vnfd`

Example:
> curl -X POST -F "vnfd=@./vnf_descriptor.tar.gz" <http://{dispatcher_host}:8082/validator/onboard/vnfd>

- Validate NS descriptor:
`/validate/nsd`

Example:
> curl -X POST -F "nsd=@./ns_descriptor.tar.gz" <http://{dispatcher_host}:8082/validator/validate/nsd>

- Validate and onboard NS descriptor:

`/onboard/nsd`

Example:
> curl -X POST -F "ns=@./ns_descriptor.tar.gz" <http://{dispatcher_host}:8082/validator/onboard/nsd>

## Responses

The *Validator* issues a response in string format along with a status code:

- 200, 201: ok
- 400: Error during the validation
- 409: Conflict with descriptor already present
- 500: Network error

## Logging

Application logs are available in the application directory as `validator.log`

#### Example

    2019-12-09 12:17:38,704 -VALIDATOR API- INFO Starting app
    2019-12-09 12:23:51,381 -VALIDATOR API- INFO Validating Experiment descriptor
    2019-12-09 12:23:51,400 -VALIDATOR API- DEBUG Experiment descriptor:
    2019-12-09 12:23:51,506 -VALIDATOR API- DEBUG Experiment descriptor sucessfully validated
    2019-12-09 12:24:07,708 -VALIDATOR API- INFO Validating Experiment descriptor
    2019-12-09 12:24:07,721 -VALIDATOR API- DEBUG Experiment descriptor:
    2019-12-09 12:24:07,803 -VALIDATOR API- DEBUG Problem while validating Experiment descriptor: '1234' is not of type 'integer'

## Next steps

- Control exceptions more efficiently

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
