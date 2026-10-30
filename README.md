# 🔐 Cybersecurity Lab Setup — Kali Linux in VirtualBox

A beginner-friendly guide to installing Kali Linux as a virtual machine using Oracle VirtualBox.

> **Purpose:** Build a safe, local environment for learning Linux and cybersecurity fundamentals. Only test systems you own or have explicit permission to assess.

## 📌 Project Overview

This project documents the installation of Kali Linux inside Oracle VirtualBox. Running Kali as a virtual machine keeps it separate from the host operating system and makes it easier to take snapshots, recover from mistakes, and build additional lab machines later.

## 🎯 Objectives

- Download VirtualBox and the official Kali Linux installer.
- Create a new Kali Linux virtual machine.
- Install Kali Linux on a virtual hard disk.
- Complete the first boot and update the system.
- Verify basic network connectivity.
- Capture screenshots documenting the actual installation.

## 🧰 Requirements

| Component | Suggested configuration |
|---|---|
| Host computer | 64-bit computer with hardware virtualization support |
| Hypervisor | Oracle VirtualBox |
| Guest OS | Kali Linux 64-bit installer (amd64, for Intel/AMD x86-64 PCs) |
| RAM assigned to VM | 2 GB minimum; 4 GB recommended if the host has enough RAM |
| Virtual CPUs | 2 |
| Virtual disk | 40 GB or more, dynamically allocated |
| Network | NAT for basic internet access |

These are practical lab settings, not claims about the hardware used. Adjust them to match your computer and available resources. Do not allocate all host RAM or CPU cores to the VM.

## 🔗 Official Downloads

- [Oracle VirtualBox downloads](https://www.virtualbox.org/wiki/Downloads)
- [Kali Linux official downloads](https://www.kali.org/get-kali/)
- [Kali Linux VirtualBox installation documentation](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
- [Kali Linux download and image verification guidance](https://www.kali.org/docs/introduction/download-official-kali-linux-images/)

Download Kali only from the official Kali website. Check the published SHA256 checksum where available before using the installer.

## 🪜 Installation Procedure

### Step 1 — Install Oracle VirtualBox

1. Open the [VirtualBox downloads page](https://www.virtualbox.org/wiki/Downloads).
2. Select the package for your host operating system (Windows, macOS, or Linux).
3. Download and run the installer.
4. Keep the standard installation options unless you have a reason to change them.
5. If the installer asks to install network interfaces or drivers, allow the installation.
6. Finish the setup and open **Oracle VirtualBox Manager**.

**Screenshot:** `screenshots/01-virtualbox-installed.png`

### Step 2 — Download the Kali Linux ISO

1. Open [kali.org/get-kali](https://www.kali.org/get-kali/).
2. Choose **Installer Images**.
3. Download the 64-bit **amd64** installer ISO for a standard Intel/AMD PC.
4. Save the ISO somewhere easy to find, such as your Downloads folder.
5. Compare its SHA256 checksum with the value published by Kali.

Do not use an ISO from an unofficial mirror or an unknown third-party website.

**Screenshot:** `screenshots/02-kali-iso-downloaded.png`

### Step 3 — Create a new virtual machine

1. Open VirtualBox and click **New**.
2. Set the name to `Kali Linux`.
3. Set the Type to **Linux** and Version to **Debian (64-bit)** (wording may vary slightly by VirtualBox version).
4. Select the downloaded Kali ISO as the installation image if the wizard offers that option.
5. If VirtualBox offers unattended installation, choose the manual/skip option so you can document the Kali installer yourself.
6. Continue to the hardware settings.

**Screenshot:** `screenshots/03-create-kali-vm.png`

### Step 4 — Configure VM resources

Use settings suitable for your computer:

- **Memory:** 4096 MB recommended (2048 MB minimum).
- **Processors:** 2 CPUs.
- **Virtual hard disk:** Create a new disk.
- **Disk type:** VDI.
- **Storage:** Dynamically allocated.
- **Disk size:** 40 GB or more.

Click **Finish/Create** to create the VM.

**Screenshot:** `screenshots/04-vm-hardware-settings.png`

### Step 5 — Configure the virtual machine

1. Select the Kali VM and open **Settings**.
2. Under **System**, confirm the boot order can start from the optical ISO before the hard disk for the first installation.
3. Under **Display**, increase video memory if the option is available (for example, 128 MB).
4. Under **Network**, leave **Adapter 1** enabled and attached to **NAT** for a simple first setup.
5. Confirm the Kali ISO is mounted in **Storage** if it was not selected in the creation wizard.
6. Save the settings.

NAT is a simple default for internet access. If you later build a multi-VM lab, design a separate private network and review its isolation before connecting target machines.

**Screenshot:** `screenshots/05-vm-settings.png`

### Step 6 — Start the Kali installer

1. Select the VM and click **Start**.
2. At the Kali boot menu, choose **Graphical install** (or the equivalent graphical installer entry).
3. Select your language, location, and keyboard layout.
4. Wait while the installer detects hardware and configures the installation environment.

**Screenshot:** `screenshots/06-kali-installer.png`

### Step 7 — Configure the system and user account

1. Enter a hostname, such as `kali-lab`.
2. Configure a domain name only if your lab requires one; otherwise leave it blank.
3. Create a normal user account and a strong password.
4. Select the appropriate time zone.
5. Continue through the installer.

Use a password that you do not reuse on other accounts.

**Screenshot:** `screenshots/07-user-and-system-setup.png`

### Step 8 — Partition the virtual disk

1. Choose **Guided — use entire disk** if you are using the new virtual disk created for this VM.
2. Carefully confirm that the selected disk is the VirtualBox virtual disk, not a physical host drive.
3. Choose the recommended partition layout for a simple lab.
4. Confirm writing the changes to the virtual disk.

**Important:** This operation should affect only the VM's virtual disk. Stop if the installer appears to be selecting a physical disk belonging to your host computer.

**Screenshot:** `screenshots/08-disk-partitioning.png`

### Step 9 — Select software and install the bootloader

1. When asked about software selection, keep the default Kali desktop environment and standard tools unless you have a specific reason to change them.
2. Allow the installer to copy and configure the packages.
3. When asked, install GRUB to the VM's virtual boot disk (normally the disk shown by the installer, such as `/dev/sda`).
4. Finish the installation and reboot.
5. If the installer asks you to remove installation media, unmount/eject the ISO from the VM's virtual optical drive so it boots from the virtual hard disk.

**Screenshot:** `screenshots/09-installation-complete.png`

### Step 10 — Log in and update Kali

1. Start the VM from its virtual hard disk.
2. Log in with the user account you created.
3. Open a terminal.
4. Refresh package information and install available updates:

```bash
sudo apt update
sudo apt full-upgrade -y
```

5. Reboot if the update process requests it:

```bash
sudo reboot
```

**Screenshot:** `screenshots/10-kali-desktop.png`

### Step 11 — Verify the installation

Open a terminal and run:

```bash
whoami
uname -a
ip address
```

Check network connectivity (these tests contact public internet hosts):

```bash
ping -c 4 1.1.1.1
ping -c 4 kali.org
```

A successful IP ping checks basic connectivity. A successful domain ping also indicates DNS resolution. If either test fails, record the error and troubleshoot the VM's network settings; do not report a test as successful unless you actually ran it.

**Screenshot:** `screenshots/11-verification-terminal.png`

### Step 12 — Take a clean snapshot

1. Shut down Kali cleanly.
2. In VirtualBox Manager, open the VM's **Snapshots** view.
3. Take a snapshot named `Fresh Kali Installation`.
4. Start the VM again if you want to continue working.

A snapshot gives you a recovery point before future lab exercises.

**Screenshot:** `screenshots/12-clean-snapshot.png`

## 🖼️ Screenshots

Add your own screenshots to the `screenshots/` folder using the exact filenames referenced above. The screenshots should show your actual VirtualBox and Kali installation, not images copied from another person's repository.

| Screenshot | What it should show |
|---|---|
| 01 | VirtualBox Manager installed/open |
| 02 | Official Kali ISO downloaded |
| 03 | New Kali VM creation screen |
| 04 | RAM, CPU, and virtual disk settings |
| 05 | VM settings including NAT and mounted ISO |
| 06 | Kali graphical installer |
| 07 | Hostname/user setup (hide personal details) |
| 08 | Guided partitioning of the virtual disk |
| 09 | Installer completion / reboot |
| 10 | Kali desktop after first login |
| 11 | Terminal showing verification commands and their real output |
| 12 | VirtualBox snapshot named `Fresh Kali Installation` |

**How to take screenshots:** On Windows use `Win + Shift + S`; on Linux use your desktop screenshot shortcut; on macOS use `Shift + Command + 4`. Crop out personal information, email addresses, hostnames you do not want public, and any sensitive data before committing screenshots.

## ⚙️ Lab Configuration Record

Fill this table with the values from your own completed VM:

| Setting | Actual value |
|---|---|
| Host operating system | _Add your host OS and version_ |
| VirtualBox version | _Add installed version_ |
| Kali version | _Add installed version_ |
| RAM assigned | _Add actual value_ |
| CPU cores assigned | _Add actual value_ |
| Virtual disk size | _Add actual value_ |
| Network mode | NAT / other: _specify_ |
| Kali IP address | _Add only if safe to publish_ |

Do not copy example values from another repository. Record what your own machine actually uses.

## 💡 What I Learned

Complete this section after doing the installation in your own words. You can mention:

- How a hypervisor runs a guest operating system inside a host operating system.
- Why a virtual disk is separate from the host's physical disk.
- How NAT provides basic network access to a guest VM.
- Why snapshots are useful before cybersecurity experiments.
- How to verify Linux version, user identity, IP configuration, and network connectivity.

## 🛡️ Security and Ethical Use

This environment is for education and authorized security testing only. Test only your own devices, your own lab, or systems for which you have explicit written permission. Keep experimental targets isolated and do not scan or attack public systems without authorization.

## 📚 References

- [Kali Linux — Kali inside VirtualBox](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
- [Kali Linux — Downloading official images](https://www.kali.org/docs/introduction/download-official-kali-linux-images/)
- [Oracle VirtualBox — Downloads](https://www.virtualbox.org/wiki/Downloads)
- [Oracle VirtualBox — Creating a virtual machine](https://docs.oracle.com/en/virtualization/virtualbox/7.2/user/create-vm.html)

---

**Project status:** Documentation template prepared. Replace the screenshot placeholders and configuration fields with evidence from your own installation before submitting this repository.
