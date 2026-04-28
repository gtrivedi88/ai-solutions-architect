## Daily Progress Log

- **Date:** Monday, April 27, 2026  
- **Day Completed:** Day 2  

---

## Topics Covered

- **Provisioning:** Created an Azure Storage Account within the `rg-frugal-dev` Resource Group.  
- **Data Management:** Created a Blob Container and performed a manual image upload.  
- **Troubleshooting:** Navigated regional availability and subscription constraints.  

---

## Learnings & Obstacles

### The Quota Hurdle
Initially, creating storage seemed impossible. Regardless of the location selected, the **"Azure for Students"** subscription repeatedly rejected requests with `RequestDisallowedByAzure`.

### The CLI Breakthrough
Transitioned to a programmatic deployment using the following CLI command:

```bash
az login
az storage account create \
  --name frugalstor2026 \
  --resource-group rg-frugal-dev \
  --location eastus \
  --sku Standard_LRS \
  --kind StorageV2 \
  --min-tls-version TLS1_2
```

### The "East US" Revelation
Through trial and error, discovered that certain data centers (like `eastus`) are more reliably "open" for student-tier accounts. Successfully created the storage via CLI, verified it in the Portal, and completed the container setup.

---

## 🧠 The "Day 2" Pressure Test: Solutions

### Scenario 1: The "Invisible" Wall
- **Problem:** `RequestDisallowedByAzure` with empty Policy Assignments.  
- **Architect Answer:** This is a subscription-level quota or provider restriction. Specific regions are often restricted for student-tier accounts to manage capacity.  

```bash
az account list-locations --query "[?metadata.regionType=='Physical'].{Name:name, DisplayName:displayName}"
```

---

### Scenario 2: The Data Sovereignty Crisis
- **Problem:** Storage account created in the wrong country (legal violation).  
- **Architect Answer:** You cannot "move" a storage account. Create a new account in the correct region and migrate data using tools like AzCopy or Python scripts.  

---

### Scenario 3: The TLS 1.0 Security Breach
- **Problem:** Enforcing modern encryption (TLS 1.2).  
- **Architect Answer:** Use the `--min-tls-version TLS1_2` flag.  

---

### Scenario 4: The Naming Collision
- **Problem:** `ai_data_storage` fails.  
- **Architect Answer:** Storage account names must be globally unique across Azure.  

**Rules:**
1. 3–24 characters  
2. Lowercase letters and numbers only  
3. No special characters (e.g., underscores)  

---

### Scenario 5: The "Hot" vs "Cold" Cost Leak
- **Problem:** Skyrocketing bills for high-volume logs.  
- **Architect Answer:** Change Access Tier from **Hot** to **Cool** or **Archive**.  

```bash
az storage account update --name <name> --access-tier Cool
```

---

### Scenario 6: The Accidental Deletion
- **Problem:** Deleting a Resource Group via `az group delete`.  
- **Architect Answer:** This is destructive. Deleting an RG deletes all resources inside it.  

---

### Scenario 7: The "Handshake" Failure
- **Problem:** `AuthenticationFailed` after 24 hours.  
- **Architect Answer:** Login tokens expire. Re-authentication is required.  

---

### Scenario 8: Scripting for Idempotency
- **Problem:** Script fails if the resource already exists.  
- **Architect Answer:** Check before creation:  

```bash
az storage account check-name --name <name>
```

---

### Scenario 9: The Private vs Public Access
- **Problem:** Image URL returns 404 or Access Denied.  
- **Architect Answer:** Public access is disabled by default. Use SAS Token or set access level to **Blob**.  

---

### Scenario 10: The Multi-Subscription Trap
- **Problem:** Multiple accounts linked to one login.  
- **Architect Answer:** Set the correct active subscription:  

```bash
az account set --subscription "Your-Subscription-ID"
```
---

**End**
