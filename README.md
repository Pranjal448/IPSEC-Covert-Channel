# IPSEC-Covert-Channel

Perfect 👍
Below is **direct README-style content** — no explanations, no commentary. You can paste this straight into `README.md`.

---

# Virtual Machine Setup and Network Configuration

## 1. Overview

This project was developed and tested using a virtualized environment to ensure isolation, reproducibility, and controlled networking. Two Ubuntu virtual machines were created on a single Ubuntu host system using native Linux virtualization tools.

---

## 2. Host System Details

* **Host Operating System:** Ubuntu Linux
* **Virtualization Interface:** virt-manager (Virtual Machine Manager)
* **Underlying Hypervisor:** KVM/QEMU

`virt-manager` is a graphical management tool for KVM-based virtual machines and uses `libvirt` to handle virtual hardware and networking.

---

## 3. Virtual Machine Creation

Two separate Ubuntu virtual machines were created to simulate a multi-node setup.

### 3.1 Guest Operating Systems

| VM Name | OS              | Architecture |
| ------- | --------------- | ------------ |
| VM1     | Ubuntu (64-bit) | x86_64       |
| VM2     | Ubuntu (64-bit) | x86_64       |

### 3.2 Common VM Configuration

* **CPU:** 1 vCPU
* **Memory:** 1–2 GB RAM
* **Disk:** 20 GB (QCOW2 format)
* **Installation Medium:** Ubuntu ISO image
* **Firmware:** BIOS (default)

Each virtual machine was installed independently using the standard Ubuntu installer.

---

## 4. Network Configuration

### 4.1 Network Type

Both virtual machines were connected using the **default NAT-based virtual network** provided by `libvirt`.

Key characteristics:

* Virtual bridge: `virbr0`
* Private subnet: `192.168.122.0/24`
* IP assignment: DHCP
* Internet access: Enabled via NAT
* VM-to-VM communication: Enabled

The host system acts as a DHCP server and network gateway for the virtual machines.

---

### 4.2 IP Address Verification

Inside each virtual machine, network details were verified using:

```bash
ip a
```

or

```bash
ifconfig
```

Example output:

```
inet 192.168.122.101/24
```

This confirms successful assignment of an IP address within the virtual network.

---

## 5. Connectivity Testing

### 5.1 VM-to-VM Communication

Connectivity between the two virtual machines was verified using ICMP echo requests.

```bash
ping <IP_of_other_VM>
```

Successful replies confirmed correct network connectivity.

---

### 5.2 VM-to-Host Communication

```bash
ping 192.168.122.1
```

This address corresponds to the host-side interface of the virtual bridge.

---

## 6. Rationale for Virtualized Setup

* Provides a controlled and isolated testing environment
* Eliminates dependency on physical hardware
* Ensures easy reproducibility of experiments
* Allows realistic simulation of networked systems

---

## 7. References

* virt-manager Documentation: [https://virt-manager.org/documentation/](https://virt-manager.org/documentation/)
* libvirt Networking: [https://libvirt.org/formatnetwork.html](https://libvirt.org/formatnetwork.html)
* Ubuntu Virtualization Guide: [https://ubuntu.com/server/docs/virtualization](https://ubuntu.com/server/docs/virtualization)

---

If you want, next we can add:

* **Step-by-step virt-manager screenshots section**
* **Static IP configuration**
* **Firewall rules (ufw / iptables)**
* **Exact commands used (audit-style README)**
