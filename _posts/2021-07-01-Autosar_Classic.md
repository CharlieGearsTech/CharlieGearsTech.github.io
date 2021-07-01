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

![](https://raw.githubusercontent.com/CharlieGearsTech/CharlieGearsTech.github.io/f7ce567ade5c4187a92a290b9686090720ea272a/images/AutosarClassic_sc1.png)
