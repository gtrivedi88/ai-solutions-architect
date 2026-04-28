## Daily Progress Log

**Date:** Monday, April 27, 2026
**Day Completed:** Day 3

### Topics Covered

* Understood **Control Plane vs Data Plane** in Azure
* Installed Azure SDK for Python (`azure-storage-blob`, `azure-identity`, `azure-mgmt-resource`)
* Connected Python to Azure using a **connection string via environment variables**
* Went to Azure (Security + Networking), found the palcement of the Key and connection string
* Accessed Azure Blob Storage programmatically
* Listed blobs inside a container using Python
* Learned difference between **resource management (control plane)** and **data interaction (data plane)**

### Key Concepts

* **Control Plane:** Used to create and manage Azure resources (resource groups, storage accounts, configurations) via Azure Resource Manager
* **Data Plane:** Used to interact with actual data inside those resources (uploading, listing, downloading blobs)

---

### Code (Data Plane — List Blobs)

```python
import os
from dotenv import load_dotenv
from azure.storage.blob import BlobServiceClient

load_dotenv ()
connection_strings = os.getenv("AZURE_CONNECTION_STRINGS")

# Debug
# Add this line to check our "pass"
print(f"DEBUG: Connection string found: {connection_strings is not None}")

# Think of this as an hotel, in hotel we have front desk. And we need to contact front-desk, because we cannot go anywhere in the hotel without talking to them first.
# This hotel is entire azure.

# Hotel Manager that knows everything about the hotel. we name is service_client, coz here service is azure (the hotel) and the client is us. we can also name it as hotel_manager but it does not look nice, hence the standard name.
service_client = BlobServiceClient.from_connection_string(connection_strings)

# Now that we have talked to the hotel manager, we need to go and check in our room. To checkin to our room, we need to know the room number, in our case the container we created inside the stroage.

container_client = service_client.get_container_client("test-container")


# Now that hotel manager is at the room, we need to ask hotel manager to show us everything in the room. In our case everythin in our container. The container (the room) had can have more than one artifact (blobs). Blobs are generallt .txt, .pdf, .png, .jpeg format. By default when you store anything on the container, it follows Blob_Type as block_blob. Which mean, before uploading your file, azure breaks your uploads into multiple smaller chunks so that if the connetion between your computer and cloud breaks,it does not have to start again from scratch.
for blob in container_client.list_blobs():
    print(blob.name)

print(f"Blob count: {len(list(container_client.list_blobs()))}")
```

---

**End**
