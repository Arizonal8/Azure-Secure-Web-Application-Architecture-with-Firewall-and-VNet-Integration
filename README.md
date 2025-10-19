# 🌩️ Azure ARM Demo — Infrastructure Deployment

This project demonstrates how to deploy Azure infrastructure using **Azure Resource Manager (ARM) templates**.  
It automates the provisioning of core cloud resources such as **Virtual Networks**, **Subnets**, and **Network Security Groups**.

---

## 📁 Project Structure

| File | Description |
|------|--------------|
| `main.json` | Main ARM template file that defines all Azure resources. |
| `vnet.json` | Nested or linked template for creating a Virtual Network and related subnets. |
| `parameters.json` | (Optional) Contains parameter values like resource names, address spaces, or regions. |
| `README.md` | Documentation for setting up and deploying this project. |

---

## 🧠 What This Template Does

The ARM template provisions:
- A **Virtual Network (VNet)** in Azure  
- One or more **Subnets** inside the VNet  
- (Optional) A **Network Security Group (NSG)**  
- All resources are deployed into the resource group `rg-demo-arinze` in the **West Europe** region.

---

## 🚀 Deployment Options

You can deploy this template using either the **Azure Portal (GUI)** or the **Azure CLI**.

---

### 🖥️ Option 1: Deploy Using Azure Portal (GUI)

1. Go to the Azure Portal: [https://portal.azure.com](https://portal.azure.com)
2. Search for **“Deploy a custom template”** or go directly to:  
   [https://portal.azure.com/#create/Microsoft.Template](https://portal.azure.com/#create/Microsoft.Template)
3. Click **“Build your own template in the editor.”**
4. Click **Load file** and select `main.json`.
5. Click **Save**, then:
   - Choose **Resource Group:** `rg-demo-arinze`
   - Choose **Region:** `West Europe`
   - Fill in parameters if prompted.
6. Click **Review + Create → Create**.

Deployment progress will appear in the top-right corner of the portal.

---

### 💻 Option 2: Deploy Using Azure CLI

Make sure you’re logged in to your Azure account and have the correct subscription set.

```bash
# Login to Azure
az login

# Validate the template
az deployment group validate   --resource-group rg-demo-arinze   --template-file main.json

# Deploy the template
az deployment group create   --resource-group rg-demo-arinze   --template-file main.json
```

---

## 🧾 Output

After successful deployment, you can view your resources in:

**Azure Portal → Resource groups → `rg-demo-arinze`**

You should see:
- Virtual Network (VNet)
- Subnet(s)
- Network Security Group (if defined)

---

## 🛠️ Prerequisites

- An active **Azure for Students** subscription  
- A resource group named `rg-demo-arinze`
- Azure CLI installed on your system  
  - [Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

---

## 📘 Learn More

- [ARM Template Documentation](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/overview)
- [Azure Quickstart Templates](https://github.com/Azure/azure-quickstart-templates)
- [Deploy Resources with Azure Portal](https://learn.microsoft.com/en-us/azure/azure-resource-manager/templates/deploy-portal)

---

### 🧑‍💻 Author
**Arinze Ihekweme**  
Student | Cloud & Cybersecurity Enthusiast  
Sheffield Hallam University — Azure for Students Subscription  
