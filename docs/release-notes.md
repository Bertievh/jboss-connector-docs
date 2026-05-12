---
sidebar_label: 'Release notes'
title: JBoss Connector release notes
description: "Version history and change details for the JBoss Connector, including new features, improvements, and bug fixes."
tags:
  - Reference
  - System Administrator
  - Automation Engineer
  - Getting Started
---

# JBoss Connector release notes

## 26

### 26.2.0

2026 May

### Fixes

:eight_spoked_asterisk: **CON-1327**: Corrected CVE-2020-36518 vulnerability.

### 26.0.0

2026 January

### Fixes

:eight_spoked_asterisk: **CON-825**: Updated JBoss client libraries to the latest versions and updated the Java environment to Java 11. This changes the JMX protocol from remoting-jmx to http-remoting-jmx.

## 19

### 19.0.0

2024 March

### What's new

:eight_spoked_asterisk: **Query**: Display the attribute value retrieved by the query operation in the job log.

:eight_spoked_asterisk: **Query**: Save the attribute value retrieved by the query operation in an OpCon property.

:eight_spoked_asterisk: **Update**: Update an attribute value associated with an mbean (the attribute must be writable).

:eight_spoked_asterisk: **SubType**: The Query tab now includes a **PropertyName** text option.

:eight_spoked_asterisk: **SubType**: A new **Attribute Update** tab is available for updating the value of an mbean attribute.

### Fixes

:eight_spoked_asterisk: **Logging**: Changed logging implementation, removing log4j due to a security vulnerability.

### What's changed

Installation changed to a simple file extraction instead of an installation program.

The Windows installation now includes the Java directory required for execution.

UNIX/Linux installations require a Java 1.8 runtime installed separately.
