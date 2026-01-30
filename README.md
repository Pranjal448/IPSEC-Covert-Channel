# IPSEC-Covert-Channel


## Virtual Machine Setup and Network Configuration

### 1. Overview

This project was developed and tested using a virtualized environment to ensure isolation, reproducibility, and controlled networking. Two Ubuntu virtual machines were created on a single Ubuntu host system using native Linux virtualization tools.

---

// steps to open virtual machine

## 2. Host System Details

* **Host Operating System:** Ubuntu Linux
* **Virtualization Interface:** virt-manager (Virtual Machine Manager)
* **Underlying Hypervisor:** KVM/QEMU

`virt-manager` is a graphical management tool for KVM-based virtual machines and uses `libvirt` to handle virtual hardware and networking.

---

## 3. Virtual Machine Creation

Two separate Ubuntu virtual machines were created to simulate a multi-node setup. One VM acts as sender VM, other acts as a receiver VM.

### 3.1 Guest Operating Systems

| VM Name | OS              | Architecture |
| ------- | --------------- | ------------ |
| VM1     | Ubuntu (64-bit) | x86_64       |
| VM2     | Ubuntu (64-bit) | x86_64       |

### 3.2 Common VM Configuration

* **CPU:** 1 vCPU
* **Memory:** 5.6 GiB
* **Disk:** 25 GB (QCOW2 format)

Each virtual machine was installed independently using the standard Ubuntu installer.

---

## 4. Network Configuration

### 4.1 Network Type

A custom isolated virtual IPv6 network named **covert** was created using libvirt via virt-manager. This network is attached to both virtual machines to provide private inter-VM communication while remaining isolated from the host system and external networks.


---

### 4.2 Manual IPv6 Configuration Using `link.sh`

#### Overview

Each virtual machine uses a shell script named `link.sh` to manually configure IPv6 networking on the isolated virtual network (`covert`).
The script is executed inside the VM to assign a **static IPv6 address** to the network interface, enabling deterministic and reproducible communication between the two VMs.

This approach avoids reliance on automatic IPv6 mechanisms such as SLAAC or DHCPv6.

---

#### Purpose of `link.sh`

The `link.sh` script performs the following high-level tasks:

1. Activates the network interface attached to the isolated network
2. Assigns a predefined static IPv6 address to the interface
3. Verifies the IPv6 configuration
4. Prepares the VM for IPv6-based communication with the peer VM

Separate scripts are used on each VM, with unique IPv6 addresses assigned to prevent conflicts.

---

#### Sender VM (VM1)

* **Network Interface:** `enp1s0`
* **Assigned IPv6 Address:** `2001:db8:100::10/64`
* **Role:** Acts as the sender in IPv6 communication tests

After configuration, VM1 is able to initiate IPv6 connectivity checks (e.g., ICMPv6 echo requests) to VM2.

---

#### Receiver VM (VM2)

* **Network Interface:** `enp1s0`
* **Assigned IPv6 Address:** `2001:db8:100::20/64`
* **Role:** Acts as the receiver in IPv6 communication tests

VM2 listens for incoming IPv6 traffic and can also initiate connectivity checks to VM1.

---

#### IPv6 Addressing Scheme

* **IPv6 Prefix:** `2001:db8:100::/64`
* **Address Type:** Static
* **Purpose:** Experimental / documentation-only IPv6 addressing

Using static addresses ensures predictable endpoints and simplifies testing and analysis.

---

#### Execution and Usage

The script is executed manually on each VM:

```bash
chmod +x link.sh
./link.sh
```

Once executed on both VMs, IPv6 connectivity can be verified using `ping6` between the assigned addresses.

---

#### Design Considerations

* Ensures full control over IPv6 addressing
* Avoids dynamic address assignment variability
* Suitable for isolated and secure VM environments
* Configuration is temporary and applies only for the current session
* Keeps permanent network configuration unchanged

---

#### Outcome

After running `link.sh` on both virtual machines:

* Each VM has a unique, known IPv6 address
* IPv6 communication is enabled over the isolated network
* VM-to-VM connectivity can be reliably tested

---

#### Note

The IPv6 configuration applied by `link.sh` is **non-persistent** and must be re-applied after a reboot. Persistent configuration would require changes to the system’s network configuration files (e.g., netplan).

---

####
