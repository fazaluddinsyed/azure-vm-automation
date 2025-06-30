# Azure VM Start/Stop Automation Using Shell Script and Azure CLI

## 📘 Project Overview

This project demonstrates how to **automate the startup and shutdown of an Azure Virtual Machine (VM)** using shell scripts and Azure CLI. The automation includes intelligent checks to verify the current state of the VM before performing any actions, helping reduce unnecessary compute costs.

---

## 🎯 Objectives

1. Create shell scripts to start and stop an Azure VM only when necessary.
2. Use Azure CLI for automation.
3. Schedule the scripts to run daily at specific times using Task Scheduler on Windows.

---

## 🛠️ Technologies Used

- **Azure CLI**
- **Shell Scripting (Bash)**
- **Azure Virtual Machines**
- **Task Scheduler (Windows)**

---

## 🗂️ Folder Structure
azure-vm-automation/
├── start-vm.sh # Starts the VM if it is not already running
├── stop-vm.sh # Stops the VM if it is not already stopped


---

## 📄 Scripts Explained

## ✅ start-vm.sh

Checks if the VM is already running. If not, starts the VM.

```bash
#!/bin/bash

resourceGroup="project1-rg"
vmName="start-stop-vm"

status=$(az vm get-instance-view --name "$vmName" --resource-group "$resourceGroup" --query instanceView.statuses[1].code -o tsv)

if [[ "$status" == "PowerState/running" ]]; then
    echo "VM is already running."
else
    echo "Starting VM..."
    az vm start --resource-group "$resourceGroup" --name "$vmName"
    echo "VM started successfully."
fi
```


## ✅ stop-vm.sh

Checks if the VM is already deallocated. If not, stops the VM.

```bash
#!/bin/bash

resourceGroup="project1-rg"
vmName="start-stop-vm"

status=$(az vm get-instance-view --name "$vmName" --resource-group "$resourceGroup" --query instanceView.statuses[1].code -o tsv)

if [[ "$status" == "PowerState/deallocated" ]]; then
    echo "VM is already stopped."
else
    echo "Stopping VM..."
    az vm deallocate --resource-group "$resourceGroup" --name "$vmName"
    echo "VM stopped successfully."
fi
