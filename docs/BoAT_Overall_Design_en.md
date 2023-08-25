# BoAT Edge Overall Design

## Introduction

### Purpose
This document describes the overall system design of BoAT Edge (referred to as BoAT hereafter), including the architectural design, subsystem functionalities, internal and external interfaces, key processes, and key technical design descriptions. It aims to guide the overall design and testing strategy/specification of each subsystem. The intended readers of this document are BoAT Edge design personnel.

### Abbreviations
|Term   |Explanation                  |
|:----- |:--------------------------- |
|ABI    |Application Binary Interface |
|BE     |BoAT-Engine                  |
|BIA    |BoAT Infra Arch              |
|BoAT   |Blockchain of AI Things      |
|BPT    |BoAT-ProjectTemplate         |
|BSL    |BoAT-SupportLayer            |
|RLP    |Recursive Length Prefix      |
|RNG    |Random Numeral Generator     |
|RPC    |Remote Procedure Call        |
|TEE    |Trusted Execution Environment|
|RAM    |Random Access Memory         |

## BoAT Edge Design Objectives
As a blockchain application for the Internet of Things (IoT) edge, BoAT Edge should be designed with minimal changes required for easy and quick porting to various IoT devices or modules. The design of BoAT Edge follows the following principles:
+ Hierarchical design
+ Support for multiple blockchain protocols
+ Scalable design
+ Key security design
+ Provision of C language interface contract generation tools for different blockchains

## BoAT Edge Position in the Entire Blockchain Network
As a middleware connecting IoT devices and the blockchain, BoAT Edge's position in the entire interaction network is shown in Figure 3-1.
![BoAT position](./images/BoAT_Overall_Design_cn-F3-1-Boat_Position.png)
Figure 3-1: BoAT's position in the blockchain interaction network
