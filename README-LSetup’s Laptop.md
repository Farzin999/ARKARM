# ARKARM
# Farzin's ARK - ARM project
# Environment setup instructions

## Azure Setup

### Task 1: Create a resource group in Azure

1. Log into the [Azure Portal](https://portal.azure.com) using your Azure credentials.

2. On the Azure Portal home screen, select the **+ Create a resource** tile.

    ![A portion of the Azure Portal home screen is displayed with the + Create a resource tile highlighted.](./artifacts/environment-setup/media/bhol_createaresource.png)

3. In the **Search the Marketplace** text box, type **Resource group** and press the **Enter** key.

    ![On the new resource screen Resource group is entered as a search term.](./artifacts/environment-setup/media/bhol_searchmarketplaceresourcegroup.png)

4. Select the **Create** button on the **Resource group** overview page.

5. On the **Create a resource group** screen, select your desired Subscription and Region. For example, enter **gb-evity-prdsyn-001** add **Tags**, then select the **Review + Create** button.

    ![The Create a resource group form is displayed populated with Synapse-MCW as the resource group name.](./artifacts/environment-setup/media/tun_resourcegroupform.png)

6. Select the **Create** button once validation has passed.

### Task 2: Pre-requisites via Local PowerShell

* Windows PowerShell
* Azure PowerShell

* `Az.CosmosDB` 0.1.4 cmdlet

    ```powershell
    Install-Module -Name Az.CosmosDB -RequiredVersion 0.1.4
    ```

* `sqlserver` module

    ```powershell
    Install-Module -Name SqlServer
    ```

 * `PowerBI` module

    ```powershell
       Install-Module -Name MicrosoftPowerBIMgmt
    ```

* update the **synapse.parameters.json** file with appropriate parameter values. File located in **./artifacts/environment-setup/parameter/**

```powershell
$AzureUserName="odl_user_NNNNNN@msazurelabs.onmicrosoft.com"
$AzurePassword="..."
$TokenGeneratorClientId="1950a258-227b-4e31-a9cf-717495945fc2"
$AzureSQLPassword="..."
```
> The `AzureSQLPassword` value is the value passed to the `sqlAdministratorLoginPassword` parameter when running the `01-asa-workspace-core.json` ARM template. You can find this value by looking at the `SQL-USER_ASA` Key Vault secret.

* update the **AzureCreds.ps1** file with appropriate parameter values. File located in **./artifacts/environment-setup/parameter/**


### Task 3: Execute setup scripts

* Open PowerShell as an Administrator and change directories to the root of this repo within your local file system.
* Run `Set-ExecutionPolicy Unrestricted`.
* or Run  `Set-ExecutionPolicy -Scope process -ExecutionPolicy Bypass`.

* Execute `Connect-AzAccount` and sign in to the ODL user account when prompted.
    ```powershell
    cd <root of this repo within your local file system>
    .\artifacts\environment-setup\automation\a00-synapse-aaa-coreNoCodmos.ps1
    .\artifacts\environment-setup\automation\a01-evity-workspace-setup.ps1
    .\artifacts\environment-setup\automation\a02-evity-environment-setup.ps1
    ```
## setup is now compelted

The entire script will take about an hour to complete.  Major steps include:

* Configure Synapse resources
* Download all data sets and files into the data lake (~15 mins)
* Execute the setup and execute the SQL pipeline (~30 mins)


## End of README file