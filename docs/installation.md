---
title: Installation
description: "Install the JBoss Connector on Windows or UNIX/Linux, configure the Connector.config file, and set up OpCon global properties for job sub-type access."
tags:
  - Procedural
  - System Administrator
  - Installation
sidebar_label: 'Installation'
---

# Installation

## What is it?

The JBoss Connector installation extracts connector files to a directory on a server where an OpCon agent is installed. The connector is available for both Windows and UNIX/Linux platforms.

- Use this guide when installing the JBoss Connector for the first time on a Windows or UNIX/Linux server
- Use this guide when configuring the `Connector.config` file to connect the connector to a JBoss Enterprise Application Platform environment

## Prerequisites

Before installing, confirm the following:

- An OpCon agent is installed on the target Windows or Linux server
- The JBoss Connector is compatible with JBoss EAP versions
- Java 11 is available (supplied with the Windows installation package; must be installed separately on UNIX/Linux as Java 1.8)
- OpCon LTS 23.0 or greater is running

## Installing the connector on Windows

To install the connector on Windows, complete the following steps:

1. Copy `SMAJbossConnector-win.zip` to the target directory on the server.
2. Extract the zip file contents to that directory.

The extracted root directory contains:

| Item | Description |
| --- | --- |
| Connector executable | The main JBoss Connector program |
| `encrypt.exe` | Utility for encrypting passwords used in `Connector.config` |
| `dfiles.exe` | Supporting utility |
| `Connector.config` | Configuration file for the connector |
| `java/` | Java 11 runtime environment required by the connector |
| `emplugins/` | Enterprise Manager job sub-type plug-in |

## Installing the connector on Linux

To install the connector on Linux, complete the following steps:

1. Copy `SMAJbossConnector-linux.zip` to the target directory on the server.
2. Extract the zip file contents to that directory.
   The extracted root directory contains the connector jar file, `cjboss.sh`, `encrypt.sh`, `dfiles.sh`, `Connector.config`, and the `emplugins/` directory.
3. Download the appropriate UNIX/Linux Java 1.8 runtime. If it is installed as a separate Java runtime, update `cjboss.sh`, `encrypt.sh`, and `dfiles.sh` to point to the Java installation path.
4. Run `chmod` to add execute permissions on `cjboss.sh`, `dfiles.sh`, `encrypt.sh`, and `/java/bin/java`.

## Job sub-type installation

The Enterprise Manager job sub-type enables the **JBoss** job type in your job definitions.

To install the job sub-type, complete the following steps:

1. Copy the plug-in file from the `emplugins/` directory in the connector installation to the `dropins/` directory of your Enterprise Manager installation.
   If `dropins/` does not exist, create it in the Enterprise Manager root directory.
2. Restart Enterprise Manager. The **JBoss** job sub-type appears under both Windows and UNIX job types.

**NOTE:** If the **JBoss** sub-type is not visible after restarting, close Enterprise Manager and reopen it using **Run as Administrator**. After this, Enterprise Manager can be used normally.

## Create global properties

The JBoss job sub-type uses a global property to locate the connector installation directory. Create one property per platform you have installed:

| Platform | Property name | Value |
| --- | --- | --- |
| Windows | `JBOSSPathWindows` | Full path to the connector root installation directory |
| Linux | `JBOSSPathUnix` | Full path to the connector root installation directory |

Create these properties in OpCon before creating any JBoss jobs.

## Configuration

The `Connector.config` file is located in the connector root installation directory. Edit it to define your JBoss environment connection settings and OpCon API credentials.

### Encrypting passwords

Passwords stored in `Connector.config` must be encrypted before use. The Encrypt utility is included in the installation.

To encrypt a value on Windows:

```
encrypt.exe -v "your-password-here"
```

The utility displays the encrypted value. Copy the encrypted output into the relevant password field in `Connector.config`.

### [CONNECTOR] settings

General connector settings.

| Property | Description | Default |
| --- | --- | --- |
| `NAME` | Display name for the connector | — |
| `CONFIGURATION` | JBoss server configuration type | `STANDALONE` |
| `MSGIN_DIR` | Path to the MSGIN directory of the associated Windows or UNIX agent | — |
| `OPCON_EVENT_USER` | OpCon user account used to submit events | — |
| `OPCON_EVENT_USER_PASSWORD` | Password for the OpCon event user. Encrypt before use | — |
| `DEBUG` | Logging verbosity | `OFF` |

### [OPCON_API] settings

Connection settings for the OpCon REST API.

| Property | Description | Default |
| --- | --- | --- |
| `ADDRESS` | Address and port of the OpCon REST API (`server:port`) | — |
| `TLS` | TLS version for API communication | `TLSv1.2` |
| `TOKEN` | OpCon REST API application token for authentication | — |

### [LOCAL] settings

Connection settings for a specific JBoss server. The section header (for example, `[LOCAL]`) must match the **Server Name** value entered in the job definition.

| Property | Description | Default |
| --- | --- | --- |
| `URL` | Address of the server hosting the target JBoss instance | — |
| `ADM_PORT` | JBoss management port for JMX requests | `9999` |
| `APP_PORT` | JBoss application port for JMS requests | `4447` |
| `APP_LIB_DIRECTORY` | Directory containing additional jar files to add to the connector classpath for EJB execution | — |
| `SECURITY` | Whether security is enabled on the JBoss environment (`Y` or `N`). Always required when using remote management or JMS access | — |
| `MGT_USER` | JMX management user for submitting JMX requests | — |
| `MGT_USER_PASSWORD` | Password for the JMX management user. Encrypt before use | — |
| `APP_USER` | Application user for submitting JMS requests | — |
| `APP_USER_PASSWORD` | Password for the application user. Encrypt before use | — |

### Example Connector.config

```
[CONNECTOR]
NAME=JBoss Connector
CONFIGURATION=STANDALONE
DEBUG=ON
MSGIN_DIR=c:\\test
OPCON_EVENT_USER=ocadm
OPCON_EVENT_USER_PASSWORD=6233426a6232353463484d3d

[OPCONAPI]
ADDRESS=DESKTOP-QMQS7D3:443
TLS=TLSv1.2
TOKEN=d62db7db-6098-4a08-8abd-2923a3418ebd

[LOCAL]
URL=192.168.1.164
ADM_PORT=9999
APP_PORT=4447
APP_LIB_DIRECTORY=C:\\test\\java
SECURITY=Y
MGT_USER=jbossadm
MGT_USER_PASSWORD=6233426a6232353463484d3d
APP_USER=jbossapp
APP_USER_PASSWORD=6233426a6232353463484d3d
```

## Security considerations

**Authentication**: The connector authenticates with JBoss EAP using JMX management user credentials (`MGT_USER` / `MGT_USER_PASSWORD`) and application user credentials (`APP_USER` / `APP_USER_PASSWORD`). It authenticates with the OpCon REST API using the application token (`TOKEN`).

**Password encryption**: All passwords in `Connector.config` must be encrypted using the Encrypt utility before being entered. Do not store unencrypted passwords in the configuration file.

**TLS**: Connector-to-OpCon-API communication uses TLS. The version is set by the `TLS` property in `[OPCON_API]`. The default is `TLSv1.2`.

**Access control**: Restrict access to `Connector.config` to System Administrators only. The file contains encrypted credentials and an API token.

## FAQs

**What Java version does the connector require?**
Windows installations require Java 11, which is supplied with the installation package. UNIX/Linux installations require Java 1.8, which must be installed separately.

**What OpCon version is required?**
The JBoss Connector requires OpCon LTS 23.0 or greater.

**Where is the `Connector.config` file located?**
The `Connector.config` file is in the root directory where you extracted the connector files.

**How do I encrypt passwords for `Connector.config`?**
Run `encrypt.exe -v "value"` on Windows or `encrypt.sh -v "value"` on UNIX/Linux. Copy the encrypted output into the relevant password field in `Connector.config`.

**What is the `[LOCAL]` section header in `Connector.config`?**
The section header (for example, `[LOCAL]`) must match the **Server Name** value entered in the JBoss job definition in Enterprise Manager. This tells the connector which server configuration to use for a given job.

## Glossary

**Global Property** — A system-wide variable in OpCon referenced using `[[property_name]]` syntax in job definitions. The JBoss Connector uses `JBOSSPathWindows` or `JBOSSPathUnix` to locate the connector installation.

**Connector.config** — The configuration file in the connector root installation directory. Defines the connector name, JBoss server connection settings, OpCon API credentials, and event submission settings.

**Encrypt utility** — A command-line tool included with the JBoss Connector that produces 64-bit encrypted output. Use it to encrypt all passwords before placing them in `Connector.config`.

**MSGIN directory** — The directory monitored by an OpCon agent for incoming event files. The JBoss Connector places retrieved JMS messages here as OpCon events.

**Job sub-type** — A connector-specific extension to the Enterprise Manager job definition interface. The JBoss job sub-type exposes the fields needed to configure JBoss operations (operation type, mbean name, server name, and so on).
