# anorlondo-enterprise-simulation
Full-stack virtual lab environment built in VirtualBox: AD domain controller, Windows client, network configuration, and role-based access control 

---

## Introduction

I created a hands-on project to strengthen my system administration and networking skills by building a fully virtualized enterprise lab environment while also providing myself with a foundation I can expand with future projects. 

---

## Project Overview

The goal was to simulate a small enterprise environment to experiment with:  

- Centralized identity management  
- File sharing and access control  
- Cross-platform networking  

### Current Environment

| VM   | OS / Role                               | Static IP       | Notes                                               |
|------|-----------------------------------------|-----------------|-----------------------------------------------------|
| VM1  | Windows Server 2019 – Domain Controller | 192.168.56.10   | AD, DNS, file sharing                               |
| VM2  | Windows 11 Enterprise Client            | 192.168.56.11   | Workstation                                         |
| VM3  | Ubuntu Server                           | 192.168.56.20   | SSH, security, web services (future projects)       |
| VM4  | Windows 11 Enterprise Client            | 192.168.56.12   | Workstation (future projects)                       |
| VM5  | Windows 11 Enterprise Client            | 192.168.56.13   | Workstation (future projects)                       |



<img width="1252" height="829" alt="myVMs" src="https://github.com/user-attachments/assets/bbb7e6c8-3af5-4f84-bf7b-4985d6825aac" />



All VMs are on an isolated host-only network to simulate a corporate LAN without affecting my physical network.



<img width="1999" height="555" alt="pingeachother" src="https://github.com/user-attachments/assets/b731c038-02f4-4b34-afaf-02ed4948b2f8" />



---

## Windows Server & Active Directory Setup

On **my domain controller**:



<img width="1058" height="698" alt="welcometoServer" src="https://github.com/user-attachments/assets/26ff8749-5c43-4e5c-8790-2f9fa3266d45" />



I installed Windows Server 2019 and configured a static IP. Then I set DNS to point to itself. After that, I installed **Active Directory Domain Services (ADDS)** and created the domain: `ad.anorlondo.local`.

Next, I created Organizational Units (OUs) for **IT**, **HR**, and **Sales** and created users and assigned them to their respective OUs (Example: `GwynAdmin` – Domain Admin) Lastly, I verified user logins and domain functionality.



<img width="1057" height="656" alt="addOUs" src="https://github.com/user-attachments/assets/1ec127ec-5b5a-44ed-a9fa-dd08779a51a5" />



<img width="998" height="536" alt="namesofemployees" src="https://github.com/user-attachments/assets/a3a29367-62b8-4a0c-bbdf-2236261e34e6" />



---

## Windows Client Integration

On **my Windows machine**:

I configured DNS to point to **DC1** and joined the domain.



<img width="723" height="407" alt="verifyingdomainjoin" src="https://github.com/user-attachments/assets/23028965-a2c3-4e7d-b72e-d853494e1d8b" />



I initially encountered an issue with username/password errors, as well as network connectivity quirks with VirtualBox host-only adapters  

After troubleshooting (matching NetBIOS names, verifying DNS, clearing cached security tokens), the client successfully joined the domain. Logging in as **GwynAdmin** confirmed that the group policies were applied.

---

## File Share & Access Management

On **the domain controller**, I created a shared folder: C:\Shared and started creating groups and setting their permissions.



<img width="580" height="66" alt="permissions" src="https://github.com/user-attachments/assets/a9166038-5326-477c-b1cd-08ccdea4308b" />


<img width="920" height="199" alt="newgroups" src="https://github.com/user-attachments/assets/aeefaaed-ed04-4173-a11e-5369c9f0d0cf" />


<img width="640" height="357" alt="givethegroupspermission" src="https://github.com/user-attachments/assets/7a9db70e-b3f1-4b49-8688-f0c195523cda" />


<img width="1182" height="856" alt="shareddriveyay" src="https://github.com/user-attachments/assets/44336067-97f4-447f-9150-7524a3975549" />



### Access Control Setup

In a real-world scenario, it would make sense to give HR and Sales write access in their folders. However, for the purposes of the lab, I wanted to try giving different permissions. So I decided on:

| Group         | SMB + NTFS Access |
|---------------|-------------------|
| IT            | Full Control      |
| Domain Admins | Full Control      |
| HR            | Read-Only         |
| Sales         | Read-Only         |

**Lessons Learned!**  
Windows uses **two layers of permissions**:

1. **SMB Share Permissions** – controls who can connect.  
2. **NTFS File Permissions** – controls what users can do inside the folder.  

Both layers must grant the intended access. After logging out and back in, the IT user recognized group membership and gained proper access.

---

## Linux Server Deployment

On **my linux machine**:

- Installed **Ubuntu Server** and set static IP: `192.168.56.20`  
- Future projects will include:
  - Web hosting  
  - SSH administration  
  - Integration with Windows services  

Connectivity from the Windows client was tested, confirming cross-platform network communication. This was pretty basic, but like I mentioned prior, this will be used further down the road for other projects.

---

Overall, this exercise has strengthened my understanding of enterprise infrastructure, virtualization, networking, and access control, and it’s a lab I can continue to build on for future projects.

---
