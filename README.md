# anorlondo-enterprise-simulation
A full-stack virtual lab environment that I created in VirtualBox 

---

## Introduction

I created a hands-on project to strengthen my system administration and networking skills by building a fully virtualized enterprise lab environment while also providing myself with a foundation I can expand with future projects. 

---

## Project Overview

The goal was to simulate a small enterprise environment to experiment with centralized identity management, file sharing and access control, and cross-platform networking.

### Current Environment

| VM   | OS / Role                               | Static IP       | Notes                                               |
|------|-----------------------------------------|-----------------|-----------------------------------------------------|
| VM1  | Windows Server 2019 – Domain Controller | 192.168.56.10   | AD, DNS, file sharing                               |
| VM2  | Windows 11 Enterprise Client            | 192.168.56.11   | Workstation                                         |
| VM3  | Ubuntu Server                           | 192.168.56.20   | SSH, security, web services (future projects)       |
| VM4  | Windows 11 Enterprise Client            | 192.168.56.12   | Workstation (future projects)                       |
| VM5  | Windows 11 Enterprise Client            | 192.168.56.13   | Workstation (future projects)                       |



<img width="1252" height="829" alt="myVMs" src="https://github.com/user-attachments/assets/48a0b949-0f00-4137-bdcf-af03f238b365" />



All VMs are on an isolated host-only network to simulate a corporate LAN without affecting my physical network.



<img width="1999" height="555" alt="pingeachother" src="https://github.com/user-attachments/assets/9b25f84b-940f-46bc-9cfc-29d4534ef909" />



---

## Windows Server & Active Directory Setup

On **my domain controller**:



<img width="1058" height="698" alt="welcometoServer" src="https://github.com/user-attachments/assets/2a122093-69de-4f79-81c1-4b12a34b0002" />



I installed Windows Server 2019 and configured a static IP. Then I set DNS to point to itself. After that, I installed **Active Directory Domain Services (ADDS)** and created the domain: `ad.anorlondo.local`.

Next, I created Organizational Units (OUs) for **IT**, **HR**, and **Sales** and created users and assigned them to their respective OUs (Example: `GwynAdmin` – Domain Admin) Lastly, I verified user logins and domain functionality.



<img width="1057" height="656" alt="addOUs" src="https://github.com/user-attachments/assets/74df6fed-9c59-441c-b382-aaaa94986b29" />



<img width="998" height="536" alt="namesofemployees" src="https://github.com/user-attachments/assets/f9a679c3-560f-4eec-8637-374d0caca976" />



---

## Windows Client Integration

On **my Windows machine**:

I configured DNS to point to **DC1** and joined the domain.



<img width="723" height="407" alt="verifyingdomainjoin" src="https://github.com/user-attachments/assets/abd2766d-56d5-49a5-96fe-9d073bba7325" />



I initially encountered an issue with username/password errors, as well as network connectivity quirks with VirtualBox host-only adapters  

After troubleshooting (matching NetBIOS names, verifying DNS, clearing cached security tokens), the client successfully joined the domain. Logging in as **GwynAdmin** confirmed that the group policies were applied.

---

## File Share & Access Management

On **the domain controller**, I created a shared folder: C:\Shared and started creating groups and setting their permissions.



<img width="580" height="66" alt="permissions" src="https://github.com/user-attachments/assets/ecf2c114-e3c0-4956-a44b-b26eac222ffb" />


<img width="920" height="199" alt="newgroups" src="https://github.com/user-attachments/assets/7dfff920-6c7e-4e3e-912d-442b1a934f55" />


<img width="640" height="357" alt="givethegroupspermission" src="https://github.com/user-attachments/assets/629cdfb2-8a47-4f58-9f66-5655b62b83ef" />


<img width="1182" height="856" alt="shareddriveyay" src="https://github.com/user-attachments/assets/48d6c6bf-b03f-4fff-a9fa-3353ca636d67" />



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
