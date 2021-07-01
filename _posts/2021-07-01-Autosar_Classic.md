---
layout: post
title: Autosar Classic
excerpt: "Overall summary of the concepts of Autosar"
modified: 7/07/2021, 9:00:24
tags: [Autosar, C]
comments: true
category: blog
---

# Introduction

**Autosar** (Automotive Open Source Architecture) is a global partnership of automotive companies focused on practices and process to magnify benefits on the ECU development area. Autosar architecture permits the handling and management of complexity of E/E architectures to increase re usability and interchange of ECUs architectures among OEMs or suppliers.

Autosar promotes architecture standardizations that yields to improvement of performance, safety, security, and environmental care for complex automotive systems. In the SW perspective, Autosar allows to:

* **Decouple Software (SW)** and Hardware(HW) by the abstraction of HW drivers and services management inside different layers of basic SW modules.
* Decouple of SW entities with different common functionalities by horizontal grouping.
* Reuse of SW that improves quality and efficiency.

An example of decouple SW is that a system without Autosar standardized architecture is highly dependent of the HW specifications, so re-usability is mostly impossible. Moreover, the SW development might be inefficient because requires low-level HW support. When “migrating” to an Autosar architecture system, SW is decoupled of the HW detailed specs and it is further divided into basic SW and application SW. This permits a high rate of re-usability that yields a high degree of efficient SW development.

![](https://raw.githubusercontent.com/CharlieGearsTech/CharlieGearsTech.github.io/main/images/AutosarClassic_sc1.png)

# Autosar Features

Autosar principal features are:

* **Architecture**: SW architecture with configurable basic SW, a middleware called RTE (Run-Time Environment) to handle the communication between the BSW and application.
* **Methodology**: ECU description formats and templates to configure processes of basic SW and application SW integration.
* **Application** interface: Standardized interface collection to communicate application in order to use them on a wide variety of automotive domains.
* **Acceptance Tests**: acceptance test to validate the behavior of Autosar requierements.

Principal challenges of Autosar are solutions to support complex software infrastructures, highly automatized complex systems, Car2X applications, hard real-time systems, high-velocity transmission, and systems with frequent interaction from sensors, actuators, and other ECUs. To support these highly demanding systems types, all Autosar implementation shall relies on 4 pillars to form the standard solution for the automotive ECU development.

* **Functional Safety**: mature safety features such as robust watchdog, safe E2E communication, and so on.
* **Efficiency**: Flexibility based on the use of Complex Device Drivers and cost-effectiveness by supporting different microcontrollers.
* **Mature application**: Established development process and improved quality due to widespread implementations.
* **Performance**: Hard real-time capabilities, event trigger support, and scalability by configurations.

## Autosar Classic vs Adaptive Autosar

Autosar architecture has two variations, Autosar Classic and Adaptive Autosar. Autosar Classic is focused on effective solutions while Adaptive Autosar is focused on flexible solutions.

Adaptive Autosar is out of scope for this introduction.

# Autosar Software Architecture

The following diagram is a simplified example of a system with AUTOSAR which involves two entities. **SWC** and **VBF**.

![](https://raw.githubusercontent.com/CharlieGearsTech/CharlieGearsTech.github.io/main/images/AutosarClassic_sc2.png)
