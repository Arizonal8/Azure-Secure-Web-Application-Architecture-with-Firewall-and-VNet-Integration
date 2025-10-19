# Azure Secure Web Application Architecture with Firewall and VNet Integration

## Overview

This project demonstrates a secure and scalable Azure web application deployment using **Virtual Networks (VNet), subnets, Network Security Groups (NSG), Azure Firewall, App Service, and Blob Storage**. The architecture ensures controlled traffic flows, strong security posture, and proper access management using **Azure RBAC** and **Managed Identities**.

The project is ideal for cloud engineers, security architects, and developers looking to implement **enterprise-grade secure web applications** on Azure.

---

## Architecture

### Components:

1. **Virtual Network (VNet)**

   * Segregates the network into multiple subnets for application and firewall.
   * Example Address Space: `10.0.0.0/16`
   * Subnets:

     * `subnet-app` → Hosts App Service integration
     * `subnet-fw` → Hosts Azure Firewall

2. **Network Security Groups (NSG)**

   * Controls traffic at the subnet level.
   * Example rules:

     * Allow HTTP/HTTPS to App subnet from Internet
     * Deny all other inbound traffic by default

3. **Azure Firewall**

   * Centralized traffic filtering and egress control.
   * Handles outbound traffic for App Service and other resources.

4. **Storage Account (Blob Storage)**

   * Secure file storage with private containers (`allowBlobPublicAccess: false`).
   * Managed Identity provides secure access from App Service.

5. **App Service & App Service Plan**

   * Hosts the web application.
   * VNet Integration ensures private network communication with resources.
   * Managed Identity used for secure authentication to Blob Storage.

6. **Azure AD Authentication**

   * Protects the web application using Azure Active Directory login.
   * Uses OpenID Connect via App Service Authentication for user sign-in.

---

## Tools and Technologies Used

| Category          | Tool / Service                                |
| ----------------- | --------------------------------------------- |
| Cloud Platform    | Microsoft Azure                               |
| Infrastructure    | Azure Resource Manager (ARM) Templates        |
| Networking        | Virtual Network, Subnets, NSG, Azure Firewall |
| Compute           | App Service, App Service Plan                 |
| Storage           | Azure Storage Account (Blob Storage)          |
| Identity & Access | Azure AD, Managed Identity, RBAC              |
| CLI / Automation  | Azure CLI (az commands)                       |
| Documentation     | Markdown (README.md)                          |

---

## How It Works

1. **Network Setup**

   * VNet is deployed with two subnets (`app` and `firewall`).
   * NSG rules are applied to allow only required traffic.

2. **Firewall Deployment**

   * Azure Firewall is deployed in the firewall subnet with public IP.
   * Configured for outbound DNAT, network rules, and secure egress.

3. **Storage Deployment**

   * Storage Account deployed with private access.
   * Blob container created for storing uploaded files.

4. **App Service Deployment**

   * App Service Plan created (Dev tier: F1/B1).
   * Web application deployed with VNet integration.
   * Managed Identity enables secure access to Blob Storage.

5. **Authentication**

   * Azure AD registered app linked to App Service.
   * Users log in securely, and web app uses RBAC for least-privilege access.

6. **Security Controls**

   * NSG restricts unwanted inbound traffic.
   * Firewall restricts outbound traffic to required endpoints only.
   * Managed Identity and RBAC avoid storing credentials in code.

---

## Deployment

1. **Deploy VNet, subnets, and NSG**

```bash
az deployment group create --resource-group rg-demo-arinze --template-file arm-templates/vnet.json --parameters @arm-templates/vnet.parameters.json
```

2. **Deploy Azure Firewall**

```bash
az deployment group create --resource-group rg-demo-arinze --template-file arm-templates/firewall.json --parameters @arm-templates/firewall.parameters.json
```

3. **Deploy Storage Account and Blob Container**

```bash
az deployment group create --resource-group rg-demo-arinze --template-file arm-templates/storage.json --parameters @arm-templates/storage.parameters.json
az storage container create --account-name <acct> --name uploads
```

4. **Deploy App Service and Plan with VNet Integration**

```bash
az deployment group create --resource-group rg-demo-arinze --template-file arm-templates/appservice.json --parameters @arm-templates/appservice.parameters.json
```

5. **Configure RBAC for App Service and Users**

```bash
az role assignment create --assignee <appPrincipalId> --role "Storage Blob Data Contributor" --scope /subscriptions/<sub>/resourceGroups/rg-demo-arinze/providers/Microsoft.Storage/storageAccounts/<acct>
```

---

## Security Considerations

* **NSG Rules:** Ensure only required ports are open; deny all others by default.
* **Firewall Rules:** Restrict egress traffic; allow only necessary services.
* **RBAC:** Assign least privilege to app and users; avoid giving everyone Contributor access.
* **Managed Identity:** Never store secrets in code; use system-assigned identities for Azure resources.

---

## Outcome

* Secure web app deployed with private network integration.
* Controlled inbound/outbound traffic via NSG and Firewall.
* Blob Storage accessible securely via App Service Managed Identity.
* Azure AD authentication enforced for web application users.

---
