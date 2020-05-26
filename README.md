# sqlserver-managed-backups
General notes on enabling SQL Server (2019) managed backups to Azure

## Considerations ##
Refer to [Important Considerations document](https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure?view=sql-server-ver15#important-considerations).

## High-level Steps ##
* Step 1: Create VNET and subnet (if they do not already exist)
* Step 2: Create a storage account and blob storage container
* Step 3: Enable private endpoint on storage account (https://docs.microsoft.com/en-us/azure/private-link/create-private-endpoint-storage-portal#create-your-private-endpoint)
	- use on-premise DNS instead of private DNS (apply hosts entry if not available)
	- test by accessing storage account privately from on-premise machine
* Step 3: Create SAS key (be sure to grant read, write, list, and delete permissions). Instructions online will not work as written. Refer to https://docs.microsoft.com/en-us/cli/azure/storage/container?view=azure-cli-latest#az-storage-container-generate-sas.
* Step 4: Enable SQL Server managed backup to Azure. Refer to https://docs.microsoft.com/en-us/sql/relational-databases/backup-restore/enable-sql-server-managed-backup-to-microsoft-azure?redirectedfrom=MSDN&view=sql-server-ver15&tabs=azure-cli.

Start with non-production instances.

## Virtual Network ##
Configure storage account to only be accessible via private network. To do so, it must be associated to a subnet.

### Organization Options ###
* Group all data workloads together.
* Group by application or application group.
* Group by SQL Server instance.

## Azure Storage Account ##
Be aware of the [scalability and performance targets](https://docs.microsoft.com/en-us/azure/storage/common/scalability-targets-standard-account).

| Resource                                                                 | Limit                      |
|:------------------------------------------------------------------------ |:---------------------------|
| Number of storage accounts per region per subscription                   | 250                        |
| Maximum storage account capacity                                         | 5 PiB                      |
| Maximum request rate per storage account (can be increased upon request) | 20,000 requests per second |
| Maximum ingress per storage account                                      | 25 Gbps                    |
