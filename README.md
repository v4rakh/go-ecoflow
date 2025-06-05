# Ecoflow API client via REST API or MQTT

This is a fork of https://github.com/tess1o/go-ecoflow.

**It contains breaking changes compared to the initial library!**

The fork is independent, git history is wiped and versions start from scratch.

EcoFlow has officially expanded their open APIs, introducing support for their Open IoT MQTT protocol. In addition, the fork now includes access to PowerOcean’s historical data, enabling comprehensive monitoring and analysis of past energy usage and system performance.

## Releases

Releases are done by simply setting a git tag prefixed with `v` on `main` branch.

## Pulling in upstream

If desired, you can pull in upstream manually:

```shell
# only required once
git remote add upstream https://github.com/tess1o/go-ecoflow.git

# checkout, pull and push
git checkout main
git pull --rebase upstream main
git push origin master
```

## Documentation for added functionality

This new platform and its functionality requires you to have a development account (see below how to retrieve necessary
access). **Your account credentials won't work**!

```go
httpClient.GetDeviceHistory(...., deviceSn)
```

```go
// IoT open MQTT example 
certInfo, _ := httpClient.GetOpenSignCertification(context.Background())

mqttConnectionConfig := &ecoflow.MqttConnectionConfig{
    CertificateAccount:  certInfo.Data.CertificateAccount,
    CertificatePassword: certInfo.Data.CertificatePassword,
    Url:                 certInfo.Data.Url,
    Port:                certInfo.Data.Port,
    Protocol:            certInfo.Data.Protocol,
    ClientId:            fmt.Sprintf("MY_APP_%s", uuid.New()),
}

mqttClient, _ := ecoflow.NewOpenMqttClient(mqttConnectionConfig, mqttClientConfig)

// connecting, publishing, is the same as the other examples host
```

## Original documentation

This is an Ecoflow API client that allows you to get parameters from your `Ecoflow devices` either
from `Ecoflow Rest API` (requires developer's `access token` and `secret token`) or from `MQTT` (requires user's `email`
and `password`, the same as in Ecoflow mobile app).

The client implemented via `Ecoflow Rest API` has much more functionality:

1. You can get a specific parameter from your Ecoflow device
2. You can get all current parameters from your Ecoflow device
3. You can change parameters for your Ecoflow device
4. You can list all linked Ecoflow Devices

The MQTT version on the other hand allows to subscribe for the device's parameters changes.

Important notice is that MQTT version requires only user's `email/password`, but using the client implemented
via `Ecoflow Rest API` requires `Access Token` and `Secret Token` which you'll need to receive from `Ecoflow` (there is
an instruction below).

The library was tested on Ecoflow Delta 2 and Ecoflow River 2.

### Installation

To get the library just use `go get` command: `go get git.myservermanager.com/varakh/go-ecoflow`

### Supported devices

Here is the list of devices for which the complete api (get and set device parameters) using Ecoflow REST API is
implemented:

1. Power Stations (regular ecoflow power stations, like Delta 2, River 2, etc)
2. Power Stations (PRO versions)
3. Smart Plug
4. PowerStream Micro Inverter
5. Smart Home Panel
6. Wave Air Conditioner
7. Glacier

### Features via Ecoflow Rest API

This is the list of features implemented using Ecoflow Rest API:

1. Get list of all linked devices
2. Get specified parameters from a device
3. Get all parameters from a device
4. Change device's settings

Basically that's all documented features the Ecoflow REST API provides

### Features via MQTT

Currently only getting device's parameters via MQTT is implemented. Please note you have to know device's serial number
in order to use MQTT api.

### Documentation

Link to official documentation: https://developer-eu.ecoflow.com/us/document/introduction

### How to get Access Token and Secret Token

This is required if you would like to use REST API. For MQTT only user's email address and password is required.\
You can skip this section if you want to use MQTT.

1. Go to https://developer-eu.ecoflow.com/
2. Click on "Become a Developer"
3. Login with your Ecoflow username and Password
4. Wait until the access is approved by Ecoflow
5. Receive email with subject "Approval notice from EcoFlow Developer Platform". May take some time
6. Go to https://developer-eu.ecoflow.com/us/security and create new AccessKey and SecretKey

### Usage example - MQTT

See [App MQTT](./examples/app_mqtt_example.go)

### Usage example - REST API

This is an example how to use this library via Ecoflow Rest API. It requires Access Token and Secret Token which you can
receive from Ecoflow.

See [REST API](./examples/rest_api_example.go)
