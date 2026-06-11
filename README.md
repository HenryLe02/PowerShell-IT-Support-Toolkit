# PowerShell-IT-Support-Toolkit
A hands-on PowerShell lab focused on IT Support administration, troubleshooting, network diagnostics, service management, and Event Log investigation.
# PowerShell IT Support Toolkit

## Project Overview

This project showcases how PowerShell can be used to perform common IT Support and Help Desk tasks in a Windows environment.

Throughout the lab, I used PowerShell to gather system information, manage local user accounts, troubleshoot Windows services, perform network diagnostics, and investigate Windows Event Logs.

The goal of this project was to develop practical PowerShell skills that can improve troubleshooting efficiency and support day-to-day administrative tasks commonly performed by IT Support professionals.

---

## Project Objectives

- Collect system information using PowerShell
- Manage local user accounts
- Troubleshoot Windows services
- Perform network diagnostics
- Investigate Windows Event Logs
- Improve troubleshooting efficiency through automation
- Develop practical PowerShell skills for IT Support roles

---

## Environment and Tools Used

- Windows 10 Home
- Windows PowerShell 5.1
- Windows Event Viewer
- Local User and Group Management
- TCP/IP Networking
- Windows Services

---

## Phase 1 – System Information Report

### Scenario

A user reported that their workstation was running slowly. Before performing troubleshooting activities, I collected basic system information to establish a baseline of the device configuration.

### Objective

Use PowerShell to gather key system information including the computer name, logged-in user account, Windows version, network configuration, and storage status.

### Steps Taken

#### Step 1 – Identify the Computer Name

I used the `hostname` command to identify the workstation name. Knowing the computer name is important when providing remote support, managing devices, or locating a machine within an organization.

```powershell
hostname
```
<img width="940" height="309" alt="image" src="https://github.com/user-attachments/assets/cabc8d31-8046-4cf5-aae2-90e699794ffd" />

#### Step 2 – Identify the Logged-in User

I ran the `whoami` command to determine which user account was currently signed in. This information is useful when verifying user identity and troubleshooting account-related issues.

```powershell
whoami
```
<img width="940" height="322" alt="image" src="https://github.com/user-attachments/assets/d4c14d85-c98b-4ba9-999b-9f3eb3377c59" />

I used the following command to verify the installed Windows edition.

```powershell
Get-ComputerInfo | Select WindowsProductName
```
<img width="859" height="270" alt="image" src="https://github.com/user-attachments/assets/8f16f808-9ad1-4eaf-8833-64fc3e7a456d" />

The output confirmed that the device was running Windows 10 Home.

#### Step 4 – Review IPv4 Network Configuration

I reviewed the IPv4 configuration of the available network adapters.

```powershell
Get-NetIPAddress -AddressFamily IPv4
```
<img width="715" height="583" alt="image" src="https://github.com/user-attachments/assets/fe3a135e-b20c-4b65-9263-10ebbc5d9a46" />

While reviewing the results, I noticed that the adapters were assigned APIPA addresses (`169.254.x.x`), which usually indicate that the device was unable to obtain an IP address from a DHCP server.

#### Step 5 – Check Storage Health and Disk Space

I reviewed storage information using PowerShell.

```powershell
Get-Volume
```
<img width="940" height="249" alt="image" src="https://github.com/user-attachments/assets/e22a9405-dcbc-489a-900a-f1fcebe13f5d" />

The output displayed available volumes, file system types, health status, and remaining disk space. I confirmed that the system drive was healthy and had sufficient free space available.

### Validation

I successfully collected key system information using PowerShell, including the computer name, active user account, Windows version, network configuration, and storage details. This information provided a useful baseline for future troubleshooting and support activities.

## Phase 2 – Local User Account Management

### Scenario

A new employee required a local account on a workstation. Before creating the account, I reviewed the existing local users and verified the current account configuration. The account lifecycle was then tested by creating, disabling, re-enabling, and removing a user account.

### Objective

The objective of this phase was to gain hands-on experience managing local user accounts with PowerShell and understand common account administration tasks performed by IT Support technicians.

### Steps Taken

#### Step 1 – Review Existing Local User Accounts

I began by reviewing the local user accounts configured on the workstation. This allowed me to identify built-in Windows accounts as well as the active user account currently present on the system.

```powershell
Get-LocalUser
```
<img width="940" height="251" alt="image" src="https://github.com/user-attachments/assets/a5cfbe60-2e94-4117-a1ff-b08587e93636" />


#### Step 2 – Create a Test User

To simulate a new employee onboarding process, I created a local account named `TestUser`. Creating local accounts is useful when preparing standalone workstations, lab environments, or devices that are not joined to a domain.

```powershell
New-LocalUser -Name "TestUser" -NoPassword
```
<img width="940" height="226" alt="image" src="https://github.com/user-attachments/assets/a76be413-2100-4d0f-b010-96f433e60c37" />


#### Step 3 – Disable a User Account

Next, I disabled the `TestUser` account to simulate a temporary account suspension. This is a common administrative task during employee offboarding, security investigations, or extended leave periods.

```powershell
Disable-LocalUser -Name "TestUser"
```
<img width="940" height="299" alt="image" src="https://github.com/user-attachments/assets/5e3405fb-350c-4832-809c-a9ac7de7b10f" />

After verifying the account status, I confirmed that sign-in access had been successfully disabled.

#### Step 4 – Re-enable the User Account

Once the account had been disabled, access was restored by re-enabling it. This step simulated a scenario where a user returns from leave or an account is reactivated after being disabled by mistake.

```powershell
Enable-LocalUser -Name "TestUser"
```
<img width="940" height="294" alt="image" src="https://github.com/user-attachments/assets/b706bacb-2a07-4e35-9573-72fbf1d33d37" />

A verification check confirmed that the account was active again and available for use.

#### Step 5 – Remove a Local User Account

Once testing was completed, the account was removed from the workstation. Removing unused accounts is an important security practice because it reduces unnecessary access and helps maintain a clean user environment.

```powershell
Remove-LocalUser -Name "TestUser"
```
<img width="940" height="272" alt="image" src="https://github.com/user-attachments/assets/1cbe2d3d-c915-47e0-9b6d-e861ac3a3308" />

A final verification confirmed that the `TestUser` account no longer appeared in the local user list.

### Validation

I successfully completed the full local account lifecycle using PowerShell. During this phase, I reviewed existing user accounts, created a new local user, disabled and re-enabled the account, and finally removed it from the workstation. Each action was verified to ensure the expected changes were applied successfully. This phase strengthened my understanding of user account administration tasks commonly performed by IT Support technicians.

## Phase 3 – Service Troubleshooting (Print Spooler)

### Scenario

A user reported that they were unable to print documents. To investigate the issue, I reviewed the status of the Print Spooler service, simulated a service interruption, and then restored the service to verify that printing functionality could be recovered.

### Objective

The objective of this phase was to understand how Windows services can be managed with PowerShell and gain hands-on experience troubleshooting service-related issues commonly encountered by IT Support technicians.

### Steps Taken

#### Step 1 – Check Print Spooler Service Status

I began by checking the current status of the Print Spooler service. Since this service manages print jobs sent to printers, verifying its status is one of the first troubleshooting steps when users report printing issues.

```powershell
Get-Service Spooler
```
<img width="940" height="178" alt="image" src="https://github.com/user-attachments/assets/18c4cbd5-b3e7-40bd-b80b-bad60f2a6d31" />

The output confirmed that the service was running normally.

#### Step 2 – Stop the Print Spooler Service

To simulate a real-world printing issue, I stopped the Print Spooler service. This allowed me to observe how service interruptions can affect user functionality and provided an opportunity to practice troubleshooting procedures.

```powershell
Stop-Service Spooler
```
<img width="940" height="250" alt="image" src="https://github.com/user-attachments/assets/fc778538-50e5-4dff-9d55-a4b8164e9890" />

A verification check confirmed that the service status changed to **Stopped**.

#### Step 3 – Start the Print Spooler Service

After confirming the service had stopped, I restarted it to restore normal operation. Restarting Windows services is a common troubleshooting technique used to resolve temporary service failures and application issues.

```powershell
Start-Service Spooler
```
<img width="885" height="256" alt="image" src="https://github.com/user-attachments/assets/106a1934-07c9-4912-977c-d155dafacf92" />


A final verification confirmed that the service returned to a **Running** state and was operating normally again.

### Validation

I successfully completed the troubleshooting process by checking the Print Spooler service, stopping it to simulate a printing issue, and starting it again to restore functionality. After each action, I verified the service status to ensure the expected changes had taken place. This confirmed that PowerShell can be used effectively to diagnose and resolve common Windows service issues.

## Phase 4 – Network Diagnostics

### Scenario

A user reported that they were unable to access websites or company resources. To investigate the issue, I used several PowerShell networking commands to review the workstation’s network configuration, test Internet connectivity, verify DNS functionality, and identify the default gateway.

### Objective

The objective of this phase was to gain hands-on experience using PowerShell for network troubleshooting and understand how common connectivity issues can be identified and analyzed in an IT Support environment.

### Steps Taken

#### Step 1 – Review IPv4 Network Configuration

I started by reviewing the IPv4 configuration of the available network adapters.

```powershell
Get-NetIPAddress -AddressFamily IPv4
```
<img width="670" height="479" alt="image" src="https://github.com/user-attachments/assets/71e99914-38b2-48b3-82f1-62e7078c6af5" />

During the review, I noticed that some adapters were assigned APIPA addresses (`169.254.x.x`), which typically indicate that a device was unable to obtain an IP address from a DHCP server.

This exercise helped me understand how PowerShell can be used to quickly identify addressing and connectivity issues.

#### Step 2 – Test Network Connectivity

Next, I tested connectivity to an external host using PowerShell.

```powershell
Test-NetConnection google.com
```
<img width="860" height="348" alt="image" src="https://github.com/user-attachments/assets/3f02dc1a-1dd8-43e6-bce4-0045c0bbcbbb" />

The results confirmed that the workstation was able to communicate successfully with Google and that Internet access was available.

The output also identified the active source IP address being used for network communication, demonstrating that a working connection existed despite other adapters displaying APIPA addresses.

#### Step 3 – Test DNS Resolution

To confirm that name resolution was functioning correctly, I performed a DNS lookup for `google.com`.

```powershell
Resolve-DnsName google.com
```
<img width="940" height="216" alt="image" src="https://github.com/user-attachments/assets/ac059ecf-6608-4d15-81ea-3853d729abc4" />

The command successfully returned both IPv4 and IPv6 addresses, confirming that the workstation could resolve domain names into IP addresses and communicate with DNS services correctly.

#### Step 4 – Check the Default Gateway

As a final step, I reviewed the default route configuration to identify the gateway responsible for forwarding traffic outside the local network.

```powershell
Get-NetRoute -DestinationPrefix "0.0.0.0/0"
```
<img width="940" height="236" alt="image" src="https://github.com/user-attachments/assets/ce2c5567-e3f8-47a6-ae6b-de9ca18804d3" />

The results confirmed that a valid default gateway was configured and available, providing a path for communication with external networks such as the Internet.

### Validation

I successfully used PowerShell to review network configuration details, test Internet connectivity, verify DNS resolution, and identify the default gateway. This phase strengthened my understanding of network troubleshooting techniques and demonstrated how PowerShell can be used to diagnose common connectivity issues in an IT Support environment.


## Phase 5 – Event Log Investigation

### Scenario

A user reported that their workstation had been experiencing unexpected errors and performance issues. To investigate the problem, I reviewed recent Windows System Event Logs and analyzed warning and error events that could indicate underlying system issues.

### Objective

The objective of this phase was to gain hands-on experience using PowerShell to investigate Windows Event Logs and understand how event data can be used during troubleshooting and incident investigation.

### Steps Taken

#### Step 1 – View Recent System Events

I began by reviewing the most recent entries in the Windows System Event Log.

```powershell
Get-EventLog -LogName System -Newest 10
```
<img width="940" height="377" alt="image" src="https://github.com/user-attachments/assets/29e04dfd-ab74-4c77-9bcd-ec63735c840f" />


This provided a general overview of recent operating system activity, including informational events, warnings, and system-related messages.

While reviewing the output, I identified several warning events generated by Win32k alongside normal operating system activity. This demonstrated how Event Logs can provide valuable insight into system behavior during troubleshooting.

#### Step 2 – Filter Warning Events

To focus on events that may require further attention, I filtered the log results to display only warning-level entries.

```powershell
Get-EventLog -LogName System -EntryType Warning -Newest 10
```
<img width="940" height="362" alt="image" src="https://github.com/user-attachments/assets/7f6ef5be-3ff4-4b2b-86af-1fbbcbc391e0" />

The filtered output highlighted several warnings related to power management, USB power usage, NTP client configuration, and system resource management. By narrowing the results to warning events, it became easier to identify issues that could potentially affect system stability or performance.

#### Step 3 – Review Error Events

As a final step, I investigated recent error-level events recorded in the System Log.

```powershell
Get-EventLog -LogName System -EntryType Error -Newest 10
```
<img width="940" height="354" alt="image" src="https://github.com/user-attachments/assets/ff39443d-ac6c-44de-96a0-1938268c7f99" />

Several errors were identified from sources including Volsnap, DCOM, Service Control Manager, and VBoxNetLwf. These events were associated with volume shadow copy operations, service failures, communication issues, and VirtualBox networking components.

Reviewing error events provided a better understanding of how Windows records system failures and highlighted the importance of Event Logs during root cause analysis and troubleshooting activities.

### Validation

I successfully reviewed recent System Event Logs, filtered warning events, and analyzed error-level entries using PowerShell. Throughout the investigation, I identified several event sources associated with operating system activity, power management, service failures, and application-related errors. This phase demonstrated how Event Logs can be used to investigate system issues and support troubleshooting efforts in a real-world IT Support environment.

## Outcome and Validation

All project objectives were completed successfully. Throughout this lab, I used PowerShell to collect system information, manage local user accounts, troubleshoot Windows services, perform network diagnostics, and investigate Windows Event Logs.

Each phase was completed and validated using PowerShell commands, allowing me to verify system configurations, account changes, service states, network connectivity, and event log data. This project demonstrated how PowerShell can be used as a practical administration and troubleshooting tool within an IT Support environment.

---

## What I Learned

Through this project, I gained hands-on experience using PowerShell to perform common IT Support and Help Desk tasks. I learned how to gather system information, manage local user accounts, troubleshoot Windows services, diagnose network connectivity issues, and analyze Windows Event Logs.

This project also improved my confidence working with the Windows command-line environment and showed me how PowerShell can simplify administrative tasks while supporting efficient troubleshooting in real-world IT Support scenarios.

---

## Full Lab Documentation

📄 **View Full Lab Document**

[View Full Project Documentation](https://github.com/HenryLe02/PowerShell-IT-Support-Toolkit/blob/main/Project-5-Powershell.pdf)
