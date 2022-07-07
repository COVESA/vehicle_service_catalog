# Container deployment configuration

This document describes the structure of the container configuration and runtime parameters.  
Mandatory field are service_uid, cmd, quotas, workingDir.  
A complete container deployment configuration file example is given below:

```YAML
service_uid: 8dc0c1d0-aac9-4d9d-89b8-fed0ac5fc7b2

defaultstate:
  filename: state.dat

# List of layer aliases
layers:
  - layer1
  - layer2
  - layer3

# Execute environments
env:
  - PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  - VAR1=test
  - VAR2

# Strartup command
cmd: hellop_world -c /config.txt

workingdir: /

# Required quotas
quotas:
  cpu: 50
  mem: 2KB
  state: 64KB
  storage: 64KB
  uploadspeed: 32Kb
  downloadspeed: 32Kb
  upload: 1GB
  download: 1GB
  temp: 32KB

# Hostname used in the container
hostname: my-container

# List of exposed IP ports
exposedports:
  - port: $ in_interval(8080,8090)
    protocol: tcp
  - port: 1515
    protocol: udp
  - port: 9000

# Allowed connection to service from another isolated network
allowedconnections:
  - serviceid : xxxx-yyyy
    port: $ in_interval(8080,8088)
    protocol: tcp
  - serviceid: xxxx-yyyy
    port: 1515
    protocol: udp

# Requested devices
devices:
  - name: camera0
    mode: rwm

# Requested resources
resources:
  - system-dbus
  - bluetooth

# Permissions for functional servers
permissions:
  # functional server name
  vis:
    # functionality name: access mode
    Attribute.Body.Vehicle.VIN: "r"
    Signal.Doors.*: "r"
  systemcore:
    Services.Restart: "w"
    SomeRole: "rw"
```

## Кey-value: `service_uid`

|               |                |
|:--------------|:---------------|
| **Hosted by** | [root element] |
| **YAML Type** | string         |
| **Mandatory** | Yes            |

Uniq uuid of the service.

```YAML
service_uid: 8dc0c1d0-aac9-4d9d-89b8-fed0ac5fc7b2
```

## Key-Object: `defaultstate`

|                        |                 |
|:-----------------------|:----------------|
| **Hosted by**          | [root element]  |
| **Mandatory keys**     | `filename`      |

Represent service tate file configuration

### default_state key-value: `filename`

|               |        |
|:--------------|:-------|
| **YAML Type** | string |
| **Mandatory** | yes   |

Container state file name

## List Object: `env`

|               |                |
|:--------------|:---------------|
| **Hosted by** | [root element] |
| **Mandatory** | no             |

List of container environment variable.

```YAML
env:
  - PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
  - VAR1=test
  - VAR2
```

## Key-value: `cmd`

|               |                |
|:--------------|:---------------|
| **Hosted by** | [root element] |
| **YAML Type** | string         |
| **Mandatory** | Yes            |

Container run commands.

## Key-value: `workingdir`

|               |                |
|:--------------|:---------------|
| **Hosted by** | [root element] |
| **YAML Type** | string         |
| **Mandatory** | Yes            |

Current working dir of the service inside container.

## Key-Object: `quotas`

|                        |                                                                                   |
|:-----------------------|:----------------------------------------------------------------------------------|
| **Hosted by**          | [root element]                                                                    |
| **Mandatory keys**     | `cpu`, `mem`                                                                      |
| **Optional keys**      | `state`, `storage`,`upload_speed`, `download_speed`, `upload`, `download`, `temp` |

Container limits.

```YAML
quotas:
  cpu: 50
  mem: 2KB
  state: 64KB
  storage: 64KB
  upload_speed: 32Kb
  download_speed: 32Kb
  upload: 1GB
  download: 1GB
  temp: 32KB
```

### quotas key-value: `cpu`

|               |                |
|:--------------|:---------------|
| **YAML Type** | int or decimal |
| **Mandatory** | yes            |

CPU limit (percents of CPU time)​.

### quotas key-value: `mem`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | yes                                 |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

Memory limit.

### quotas key-value: `state`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

Maximum size of the state file for container.

### quotas key-value: `storage`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

Size of the storage that accessable by container.

### quotas key-value: `uploadspeed`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

Max upload speed per second.

### quotas key-value: `downloadspeed`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

Max download speed per second.

### quotas key-value: `upload`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

The maximum amount of upload internet traffic that container can utilize per 24h.

### quotas key-value: `download`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

The maximum amount of download internet traffic that container can utilize per 24h.

### quotas key-value: `temp`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |
| **Lark grammar** | [positive integer]["KB"|"MB"|"GB"]  |

The maximum size of temporary folder for container.

## Key-value: `hostname`

|               |                |
|:--------------|:---------------|
| **Hosted by** | [root element] |
| **YAML Type** | string         |
| **Mandatory** | no             |

```YAML
hostname: my-container
```

## list Object: `exposedports`

|                        |                 |
|:-----------------------|:----------------|
| **Hosted by**          | [root element]  |
| **Mandatory keys**     | `port`          |
| **Optional keys**      | `protocol`      |

List of exposed by container ports and protocol.

```YAML
exposedports:
  - port: $ in_interval(8080,8090)
    protocol: tcp
  - port: 1515
    protocol: udp
  - port: 9000
```

### exposedPorts key-value: `port`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | in_interval                         |
| **Mandatory**    | yes                                 |

Range of exposed ports.

### exposedPorts key-value: `protocol`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |

Supported protocols tcp or udp.

## list Object: `allowedconnections`

|                        |                     |
|:-----------------------|:--------------------|
| **Hosted by**          | [root element]      |
| **Mandatory keys**     | `serviceid`, `port` |
| **Optional keys**      | `protocol`          |

List of allowed connection to another isolated containers network.

```YAML
allowedconnections:
  - service_uid : xxxx-yyyy
    port: $ in_interval(8080,8088)
    protocol: tcp
  - service_uid: xxxx-yyyy
    port: 1515
    protocol: udp
```

### allowedconnections key-value: `service_uid`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | yes                                 |

Container uid to which connection is allowed.

### allowedconnections key-value: `port`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | in_interval                         |
| **Mandatory**    | yes                                 |

Range of ports which containee allow to cinnetct outside of isolated network.

### allowedconnections key-value: `protocol`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | no                                  |

Supported protocols tcp or udp.

## list Object: `devices`

|                        |                     |
|:-----------------------|:--------------------|
| **Hosted by**          | [root element]      |
| **Mandatory keys**     | `name`              |
| **Optional keys**      | `mode`              |

List of device allias that will be accessable for container.

```YAML
devices:
  - name: camera0
    mode: rwm
```

### devices key-value: `name`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | yes                                 |

System host device allias.

### devices key-value: `mode`

|                  |                                     |
|:-----------------|:------------------------------------|
| **YAML Type**    | string                              |
| **Mandatory**    | yes                                 |

Access mode, read, write, modify.

## list key: `resources`

List of resource allias that will be accessable for container.

```YAML
resources:
  - system-dbus
  - bluetooth
```

## list key-object: `permissions`

```YAML
# Permissions for functional servers
permissions:
  # functional server name
  vis:
    # functionality name: access mode
    Attribute.Body.Vehicle.VIN: "r"
    Signal.Doors.*: "r"
  systemCore:
    Services.Restart: "w"
    SomeRole: "rw"

```

List of allowed functional servers permissions for the container.
