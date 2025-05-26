# AusCAT VM Setup on Windows Server

This guide provides a step-by-step process to configure and install Virtual Machines (VMs) on a **Windows Server** using **Hyper-V**. These VMs are essential for hosting the AusCAT software stack, including the KeyDB and CatDB components.

---

## Installing Hyper-V (if not already enabled)

If Hyper-V is not yet enabled on the Windows Server, follow these steps:

1. Right-click on the **Windows Start** button and select **Apps and Features**.
2. Select **Programs and Features** under **Related Settings**.
3. Select **Turn Windows Features on or off**.
4. Check the **Hyper-V** option (including **Hyper-V Management Tools** and **Hyper-V Platform**).
5. Click **OK** and restart the server when prompted.

---

## 1. Creating a New Virtual Machine

### Step 1: Open Hyper-V Manager
- Press `Win + R`, type **virtmgmt.msc**, and hit **Enter**.
- Alternatively, search for **Hyper-V Manager** in the Start Menu.

### Step 2: Create a New VM
1. In the left pane, right-click on your server name and select **New** > **Virtual Machine**.
2. Click **Next** in the wizard to proceed.

### Step 3: Specify VM Name and Location
- Enter a **Name** (e.g., `KeyDB-VM` or `CatDB-VM`).
- Specify the **location** for VM files or keep the default.
- Click **Next**.

### Step 4: Specify Generation
- Select **Generation 2** for modern features (recommended for Windows Server 2016/2019/2022).
- Click **Next**.

### Step 5: Assign Memory
- Allocate **RAM** according to the specifications:
  - KeyDB VM: **8 GB**
  - CatDB VM: **16 GB**
- Enable **Dynamic Memory** if necessary.
- Click **Next**.

### Step 6: Configure Networking (Attach Virtual Switch)
- Choose an existing **Virtual Switch** (e.g., `Default Switch`).
- This step simply **attaches** the VM to a virtual switch but does not finalize the network configuration.
- Click **Next**.

### Step 7: Attach Boot Media (ISO File)
- This step is about connecting the VM to the installation media, not the actual OS installation.
- Choose **Install an operating system from a bootable image file**.
- Browse to your **Ubuntu 22.04 ISO**.
- Click **Next**.

### Step 8: Complete the Wizard
- Review the settings and click **Finish**.

---

## 2. Configuring VM Settings

Before configuring the VM settings, please refer to the [Infrastructure Requirements](./INFRASTRUCTURE.md) page to ensure the VM is configured with consistent settings for vCPUs, RAM, disk size, etc. based on the recommended specifications.

### Step 1: Access Settings
- Right-click on the created VM and select **Settings**.

### Step 2: Adjust CPU Configuration
- Navigate to **Processor**.
- Increase the **Number of Virtual Processors** to **4** for KeyVM and **8** or higher for CatVM.

### Step 3: Enable Virtualization Extensions
- Go to **Processor** > **Compatibility**.
- Check **Migrate to a physical computer with a different processor** (if needed).

### Step 4: Finalize Network Adapter Configuration (Connect to Virtual Switch)
- In the previous VM creation step, you attached the VM to a virtual switch. Here, you can verify and adjust this configuration.
- Go to **Network Adapter**.
- Ensure the correct **Virtual Switch** is selected (e.g., **ExternalSwitch** or **Default Switch**).

### Step 5: Finalize Settings
- Apply and close the settings window.

---

## 3. Power On and Install the Operating System

This is the step where the actual operating system installation occurs, using the ISO you attached earlier.

### Step 1: Start the VM
- Right-click the VM and select **Connect**.
- Click **Start** from the Virtual Machine Connection window.

### Step 2: Install the OS
- Follow the standard installation process for **Windows Server** or **Ubuntu Desktop**.
- Configure basic settings: **Language**, **Region**, **Keyboard Layout**.
- Set a **strong admin password**.

### Step 3: Network Configuration
- Verify that the VM is connected to the internet.
