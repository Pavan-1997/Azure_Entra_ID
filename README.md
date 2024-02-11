# Azure_Entra_ID

Azure Active Directory (AAD)" earlier called and now as  "Azure Entra ID." Azure Active Directory is Microsoft's cloud-based identity and access management service, which helps organizations manage users and provide secure access to resources like applications and data. With Azure Active Directory, users can sign in and access various Microsoft services such as Office 365, Azure, and many others using a single set of credentials. 

## Implementation: Accessing Storage account as an Managed identity from Virtual machine 

1. Goto portal.azure.com and create a Resource Group with name - managed-identity-poc and click on Review + create


2. Create a Storage Account 

    Resource group - Selected the above created one
    
    Storage account name - managedsapavan
    
    Click on Review + create


3. Now in the created Storage account -> On the left click on Containers -> Click on + Create -> Name - test

    In the above test container -> Upload any HTML file 


4. Now go to Virtual machine -> Click on + Create -> Azure virtual machine

    Virtual machine name - manageddemovm
    
    Click on Review + create


4. Now in the created Virtual machine -> On the left click on Identity -> Toggle Status to On and click on Save


5. Now go back to the Storage account created -> Click on Access Control (IAM) ->. Click on Add role assignment -> Search for Storage Blob Data Owner -> Click on Next -> Assign access to - Managed identity -> Click on + Select members -> Managed identity - Virtual machine (1) -> Select the created Virtual machine -> Click on Select -> Click on Review + assign 


6. Now connect to the VM from the terminal  and Fetching the deatils of the Managed Identity
   
```
sudo apt update
```

```    
sudo apt install jq
```
 Fetch the access token -access_token=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com%2F' -H Metadata:true | jq -r '.access_token')
    

8. Access the blob from Virtual Machine

```
curl "https://$managedsapavan.blob.core.windows.net/test/$index.html" -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer $access_token"
```
