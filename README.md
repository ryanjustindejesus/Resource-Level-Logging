<h1>Resource Level Logging</h1>
<b>This tutorial outlines the configuration of storage accounts, key vaults, and log analytics workspace</b>

<h2>Environments and Technologies Used</h2>

- <b>Microsoft Azure</b> 
- <b>Storage Accounts</b>
- <b>Key Vault</b>
- <b>Log Analytics Workspace</b>

<h2>Operating Systems</h2>

- <b>Windows 10</b>

<h2>Configuration Steps</h2>

![image](https://github.com/user-attachments/assets/988937b6-d548-4d27-93b2-d2881dd2f43e)
- <b>Navigate to storage acounts and click diagnostic settings</b>
- <b>Select disabled for blob and add diagnostic setting</b>

![image](https://github.com/user-attachments/assets/ad845ad9-5948-42c4-be3a-9255daa39750)
- <b>Select audit and send to Log Analytics Workspace</b>
- <b>Subscription: Azure Subscription 1</b>
- <b>Log Analytics Workspace: LAW-Cyber-Lab-01 (eastus2)</b>
- <b>Click save</b>

![image](https://github.com/user-attachments/assets/78422ae5-8afa-4e09-ae9a-26c37a85efc4)
- <b>Navigate to key vault and create a key vault</b>
- <b>Subscription: Azure Subscription 1</b>
- <b>Resoure Group: RG-Cyber-Lab</b>
- <b>Key vault name: akv-cyber-lab-12345</b>
- <b>Region: East US and click next</b>

![image](https://github.com/user-attachments/assets/777578b6-507c-4dfd-9bda-aaaf042a2fca)
- <b>Select vault access policy and click review + create</b>

![image](https://github.com/user-attachments/assets/dec383e1-9673-4f3d-96b5-b8367b7b729f)
- <b>Click akv-cyber-lab-12345</b>
- <b>Select secrets and click generate/import</b>

![image](https://github.com/user-attachments/assets/78147e44-9fa4-44e8-aa4a-2629b56890d4)
- <b>Name: Tenetl-Global-Admin-Password</b>
- <b>Secret value: cyberlab12345</b>
- <b>Click create</b>

![image](https://github.com/user-attachments/assets/b8399d7a-32e5-4477-a7ba-b067d2150590)
- <b>Navigate to key vaults and select akv-cyber-lab-12345
- <b>Select diagnostic settings and click add diagnostic setting</b>

![image](https://github.com/user-attachments/assets/ded5e5e1-3174-4465-bfa2-0a6a4b38b6fc)
- <b>Diagnostic setting name: ds-akv</b>
- <b>Select audit and send to Log Analytics Workspace</b>
- <b>Subscription: Azure Subscription 1</b>
- <b>Log Analytics Workspace: LAW-Cyber-Lab-01 (eastus2)</b>
- <b>Click save</b>

![image](https://github.com/user-attachments/assets/9f82ceca-a38d-4b38-8a10-56e7a345bc97)
- <b>Navigate to key vaults and click akv-cyber-lab-12345</b>
- <b>Select secrets and click generate/import</b>
- <b>Name: Super-Secret-Password-1</b>
- <b>Secret value: cyberlab12345</b>

![image](https://github.com/user-attachments/assets/f55787a0-119c-49d6-b628-4fb1a5b6ab13)
- <b>Navigate to storage accounts and select sacyberlab1234</b>
- <b>Name: test</b>
- <b>Click create</b>

![image](https://github.com/user-attachments/assets/0e7b2357-fbf1-4745-aaa0-f12db3c92ef2)
- <b>Click test container and select upload</b>

![image](https://github.com/user-attachments/assets/76296e61-8370-434a-a532-ba4da57cdb9a)
- <b>Upload a random text file and click save</b>

![image](https://github.com/user-attachments/assets/a9b8cf8b-3650-48d2-bddc-6cf86dd12b56)
- <b>Navigate to Log Analytics Workspace</b>
- <b>Use this query to observe storage blob logs:</b>
``` 
StorageBlobLogs 
```

![image](https://github.com/user-attachments/assets/9132d9b7-5104-4c29-a9f8-06c94dac7485)
- <b>Use this query to count Getblob storage accounts</b>
``` 
StorageBlobLogs
| where OperationName == "GetBlob"
| summarize count = count() by OperationName, CallerIpAddress
| where Count >= 1 
```

![image](https://github.com/user-attachments/assets/cda44978-bbc0-4bf5-ba70-ff9f191f6329)
- <b>Navigate to storage accounts and click test container</b>
- <b>Delete the random text file</b>

![image](https://github.com/user-attachments/assets/8cd392c8-22f0-401e-8028-dc7c81ddd47c)
- <b>Navigate to Log Analytics Workspace and use this query to observe the deleted blob from the storage accounts:</b>
``` 
//Deleting a bunch of blobs (in a short time period)
StorageBlobLogs
| where OperationName == "DeleteBlob"
| where TimeGenerated > ago(30m)
| summarize Count = count() by OperationName, CallerIpAddress
| where Count >=1
```

![image](https://github.com/user-attachments/assets/a721fae1-66db-4a52-95c3-613c05daf8e9)
- <b>Use this query to observe the key vault logs:</b>
``` 
AzureDiagnostics
```
