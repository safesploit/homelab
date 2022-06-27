# Homelab
Documentation of my homelab


# Table of Contents

- [Hardware](#hardware)
  - [Intel NUC](#intel-nuc)
  - [Router](#router)
  - [Switch](#switch)
- [Network Topology](#network-topology)
- [Network Design](#network-design)
  - [Network Sandboxing](#network-sandboxing)
- [Virtualisation](#virtualisation)
  - [QEMU](#qemu)



# Hardware

## Intel NUC

  - Intel NUC Kit NUC8i5BEH
    - [i5-8259U](https://www.intel.co.uk/content/www/uk/en/products/sku/135935/intel-core-i58259u-processor-6m-cache-up-to-3-80-ghz/specifications.html): 4 cores 8 threads.
    - 32GB (2x16GB) DDR4 2400MHz CL16 16-16-16-39 latency
    - [Kingston Data Center DC1000B](https://www.kingston.com/unitedkingdom/en/ssd/dc1000b-data-center-boot-ssd) (SEDC1000BM8/480G)

## Router

  - [TP-LINK Archer C3200](https://www.tp-link.com/uk/home-networking/wifi-router/archer-c3200/) (Internal; partially sandboxed)
  - [TP-LINK Archer AX10](https://www.tp-link.com/us/home-networking/wifi-router/archer-ax10/) (Fully sandboxed)

## Switch

  - TP-Link TL-SG108S (Used within SuperHub LAN; 2021)
  - TP-LINK TL-SG108 (Used within C3200 LAN)


# Network Topology


<p align="center">
  <img width="661" alt="Network Topology" src="https://user-images.githubusercontent.com/10171446/175964935-10c1b26e-8487-473c-88d7-18ed70899f55.png">  
  </br>
  <b></b>
</p>

# Network Design


## Network Sandboxing
Used to mitigate attack surface from a malicious device spreading a worm

# Virtualisation

At present The following VMs are operated:

  - VM-01: Production Web Servers
  - VM-02: Testing Web Servers
  - VM-03: NextCloud Server
  - VM-04: DNS Server
  - VM-05: MySQL Server

Each VM with their own static internal IPv4 address.
The router has an IP address pool range reserved for the Intel NUC within 192.168.5.235-245.


## QEMU
Reason for choosing QEMU over VirtualBox or bare-metal...


# Management

## Automation

## Remote Login

## Logging


# Public Key Infrastructure

## Self-Signed Root Authority

[Self-Signed-RootAuthority](https://github.com/safesploit/Self-Signed-RootAuthority)

SWS Root CA X2 is the root authority for internally signed certificates as well as externally, such as [cloud.private.safesploit.com](https://cloud.private.safesploit.com/).

Do make note that an intermediate authority is used. Hence, the wording of root authority.


<p align="center">
  <img width="471" alt="X509v3 Trust Chain" src="https://user-images.githubusercontent.com/10171446/175967371-31d22968-3f41-4571-91bf-d473751f00ca.png">
  </br>
  <b></b>
</p>

For users without the `SWS Root CA X2` certificate installed, another domain using Let's Encrypt is used, [cloud.safesploit.com](https://cloud.private.safesploit.com/). Both `cloud.private.` and `cloud.` subdomains have similar configurations, as confirmed by [SSL Labs](https://www.ssllabs.com/ssltest/). Except `cloud.private.` subdomain has been hardened to use a smaller set of more _modern ciphers_ which results in less compatibility with older web browsers.


<p align="center">
  <img width="866" alt="X509v3 Trust Chain" src="https://user-images.githubusercontent.com/10171446/175969094-7398a52b-fabe-4f05-90a1-069e62fdcb24.png">  </br>
  <b>A sign certificate ultimately trusted by `SWS Root CA X2` being used by the internal domain `valhalla.sws-internal`</b>
</p>
