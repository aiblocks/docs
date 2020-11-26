---
title: AiBlocks Software
replacement: https://www.aiblocks.io/tools
---
# Software

Pre-built software and services you can run on your own infrastructure, provided by AiBlocks.io.

## [AiBlocks Core](../aiblocks-core/software/admin.md)
AiBlocks Core is the backbone of the AiBlocks network and does the hard work of validating and agreeing on the status of every transaction with other instances of Core through the AiBlocks Consensus Protocol (SCP).

## [Millennium](https://github.com/aiblocks/go/tree/master/services/millennium)
Millennium is the client-facing API server for the AiBlocks ecosystem. It acts as the interface between AiBlocks Core and applications that want to access the AiBlocks network. If you are running AiBlocks Core, you will probably also want to run Millennium.

## [Federation Server](https://github.com/aiblocks/go/tree/master/services/federation)
A standalone Federation protocol server designed to be dropped in to your existing infrastructure. It can be configured to pull the data it needs out of your existing SQL database.

| Platform       | Latest Release                                                                         |
|----------------|------------------------------------------------------------------------------------------|
| Mac OSX 64-bit (amd64) | [federation-darwin-amd64](https://github.com/aiblocks/go/releases/download/federation-v0.2.0/federation-v0.2.0-darwin-amd64.tar.gz)      |
| Linux 64-bit (amd64)  | [federation-linux-amd64](https://github.com/aiblocks/go/releases/download/federation-v0.2.0/federation-v0.2.0-linux-amd64.tar.gz)       |
| Linux 32-bit (arm)  | [federation-arm-amd64](https://github.com/aiblocks/go/releases/download/federation-v0.2.0/federation-v0.2.0-linux-arm.tar.gz)       |
| Windows 64-bit (amd64) | [federation-windows-amd64.exe](https://github.com/aiblocks/go/releases/download/federation-v0.2.0/federation-v0.2.0-windows-amd64.zip) |

## [Bridge Server](https://github.com/aiblocks/go/tree/master/services/bridge)
AiBlocks’s Bridge server provides a simple interface for the AiBlocks network. It is meant to simplify compliance operations and other more complicated integrations. Because it stores and manages keys and account information, access to it should be well protected. Unlike Millennium, it should never be exposed to the public internet.

## [Archivist](https://github.com/aiblocks/go/tree/master/tools/aiblocks-archivist)
This is a small tool, written in Go, for working with aiblocks-core history archives directly. It is a standalone tool that does not require aiblocks-core, or any other programs.


| Platform       | Latest Release                                                                        |
|----------------|------------------------------------------------------------------------------------------|
| Mac OSX 64-bit (amd64) | [aiblocks-archivist-darwin-amd64](https://github.com/aiblocks/go/releases/download/aiblocks-archivist-v0.1.0/aiblocks-archivist-v0.1.0-darwin-amd64.tar.gz)      |
| Linux 64-bit (amd64)   | [aiblocks-archivist-linux-amd64](https://github.com/aiblocks/go/releases/download/aiblocks-archivist-v0.1.0/aiblocks-archivist-v0.1.0-linux-amd64.tar.gz)       |
| Linux 32-bit (arm)   | [aiblocks-archivist-linux-arm](https://github.com/aiblocks/go/releases/download/aiblocks-archivist-v0.1.0/aiblocks-archivist-v0.1.0-linux-arm.tar.gz)       |
| Windows 64-bit (amd64) | [aiblocks-archivist-windows-amd64.exe](https://github.com/aiblocks/go/releases/download/aiblocks-archivist-v0.1.0/aiblocks-archivist-v0.1.0-windows-amd64.zip) |
