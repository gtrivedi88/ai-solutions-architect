## Daily Progress Log

- **Date:** Monday, May 3, 2026  
- **Day Completed:** Day 5

---

## Topics Covered

- I wrote the code again, from scratch, to connect to Azure storage account
- I then used the count to counter the files in my storage account
- And then I added the logic so that if files in my storage account are greater than 3, I should get a crowded error
- The count (file_count) logic was added before I initialized the for loop
- Then in the for loop, I added the logic to increase count by one everytime the loop reviews a file.


```python
# Task is to display azure storage in this program
import os

from azure.storage.blob import BlobServiceClient


connection_strings = os.getenv("AZURE_CONNECTION_STRINGS")



# I need to validate

service_client = BlobServiceClient.from_connection_string(connection_strings)

# Now I need to go inside the container

container_client = service_client.get_container_client("test-container")

# Show eveything in container
file_count = 0
for blob in container_client.list_blobs():

    print(blob.name)

    if blob.name.endswith(".tmp"):
        print("This is a temporary file.")
    file_count = file_count + 1


if file_count > 0:
    print("This space is getting crowded")
```

**End**
