# OS-Jackfruit: Container Runtime with Kernel Monitoring

## 📌 Overview

This project implements a simple container runtime along with a Linux kernel module for monitoring container-related activities. It allows users to start, stop, list, and log containers, while also integrating kernel-level monitoring through a device interface.

---

## 🚀 Features

### 1. Container Runtime

* Start containers using custom root filesystem
* Stop running containers
* List all containers (`ps`)
* Track container states (running, stopped, etc.)

### 2. Logging System

* Logs stored in `logs.txt`
* Tracks:

  * Container start events
  * Container stop events
* View logs using:

  ```bash
  ./boilerplate/engine logs <container_id>
  ```

### 3. State Management

* Maintains container metadata in `containers.txt`
* Stores:

  * Container ID
  * PID
  * State

### 4. Kernel Module

* Custom kernel module: `monitor.ko`
* Registers device: `/dev/container_monitor`
* Enables interaction between user-space runtime and kernel

---

## 🛠️ Build Instructions

### Step 1: Compile User-Space Runtime

```bash
make -C boilerplate ci
```

### Step 2: Compile Kernel Module

```bash
make
```

---

## ⚙️ Load Kernel Module

```bash
sudo insmod monitor.ko
sudo dmesg | tail
```

Note the major number from logs, then create device:

```bash
sudo mknod /dev/container_monitor c <major> 0
sudo chmod 666 /dev/container_monitor
```

---

## ▶️ Usage

### Start Container

```bash
./boilerplate/engine start <id> <rootfs> "<command>"
```

Example:

```bash
./boilerplate/engine start c1 ./rootfs-alpha "/bin/sleep 10"
```

---

### Stop Container

```bash
./boilerplate/engine stop <id>
```

---

### List Containers

```bash
./boilerplate/engine ps
```

---

### View Logs

```bash
./boilerplate/engine logs <id>
```

---

## 📂 Project Structure

```
OS-Jackfruit/
│
├── boilerplate/
│   ├── engine.c
│   ├── monitor.c
│   └── Makefile
│
├── containers.txt
├── logs.txt
└── README.md
```

---

## ⚠️ Notes

* Kernel warnings about unsigned modules are expected.
* Unused function warnings can be ignored.
* Device file must have proper permissions (`666`) for runtime to work.

---

## ✅ Conclusion

This project demonstrates:

* Process management in user space
* File-based state tracking
* Logging system implementation
* Linux kernel module integration
* Device interface communication

---


