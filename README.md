<h1>Azure CLI Project: Resource Group & Storage Account Management</h1>

<hr />

<h2>Description</h2>
<p>
  This project demonstrates hands-on use of <b>Azure CLI</b>, <b>Bash scripting</b>, and <b>cloud infrastructure automation</b> to provision and manage Azure resources programmatically.
  It was completed as part of an advanced Microsoft Learn module and includes key CLI-based workflows for real-world cloud deployment.
</p>

<hr />

<h2>Languages and Utilities Used</h2>
<ul style="margin-left: 20px;">
  <li><b>Microsoft Azure</b></li>
  <li><b>Bash Shell (macOS + Azure Shell)</b></li>
  <li><b>Azure CLI</b></li>
  <li><b>Resource Manager</b></li>
  <li><b>Storage Account Services</b></li>
</ul>

<hr />

<h2>Environments Used</h2>
<ul style="margin-left: 20px;">
  <li><b>Azure Cloud Shell</b></li>
  <li><b>macOS Terminal</b></li>
</ul>

<hr />

<h2>Program Walk-through</h2>

<h3 align="left">🔎 Viewing Active Azure Account & Available Regions</h3>

<p align="left">
  As part of the setup, I verified my active Azure subscription using <code>az account show</code> and explored all available Azure regions using <code>az account list-locations</code>. <br />
  This ensured that I was authenticated correctly and gave me insight into where I could deploy resources for optimal performance and redundancy.
</p>

<pre>
<code>
az account show --output table
az account list-locations --output table
</code>
</pre>

<p align="left">
  <img src="https://imgur.com/ix0xhvB.png" alt="Azure CLI output showing account info and available regions" width="80%" />
</p>

<p align="left">
  These commands helped confirm that I was working in a valid, active environment and guided my decision to deploy resources in <strong>westus2</strong>.
</p>

<hr />

<h3 align="left">📁 Creating a Resource Group with Dynamic Naming</h3>

<p align="left">
  To begin deploying resources in Azure, I created a resource group using Azure CLI and Bash. I used <code>$RANDOM</code> to generate a unique name for the group, allowing me to test and repeat deployments without naming conflicts.
</p>

<pre>
<code>
let "randomIdentifier=$RANDOM*$RANDOM"
location="westus2"
resourceGroup="msdocs-rg-$randomIdentifier"

az group create --name $resourceGroup --location $location --output json
</code>
</pre>

<p align="left">
  <img src="https://imgur.com/FzVfQJb.png" alt="Creating an Azure resource group using dynamic Bash variables" width="80%" />
</p>

<p align="left">
  This command successfully created a new resource group in the <strong>westus2</strong> region, establishing a container for managing related resources in the project.
</p>

<hr />

<h3 align="left">💾 Creating a Storage Account with Azure CLI</h3>

<p align="left">
  After creating the resource group, I proceeded to create a <b>StorageV2</b> account within it. Using the <code>$RANDOM</code> variable again, I ensured that the storage account name was globally unique — a requirement for Azure Storage services.
</p>

<pre>
<code>
let "randomIdentifier=$RANDOM*$RANDOM"
location="westus2"
resourceGroup="msdocs-rg-<your-actual-group-id>"
storageAccount="msdocssa$randomIdentifier"

echo "Creating storage account $storageAccount in resource group $resourceGroup"

az storage account create \
  --name $storageAccount \
  --resource-group $resourceGroup \
  --location $location \
  --sku Standard_RAGRS \
  --kind StorageV2 \
  --output json
</code>
</pre>

<p align="left">
  <img src="https://imgur.com/ksnoqSw.png" alt="Creating an Azure Storage Account with Bash and Azure CLI" width="80%" />
</p>

<p align="left">
  The terminal output confirmed successful provisioning of the storage account under the previously created resource group.
</p>

<hr />

<h3 align="left">📋 Verifying Storage Account Deployment</h3>

<p align="left">
  To confirm that my storage account was successfully created, I ran the following Azure CLI command:
</p>

<pre>
<code>
az storage account list --output table
</code>
</pre>

<p align="left">
  This command returns a tabular summary of all storage accounts in the current subscription. It displays important details such as account name, resource group, region, replication type, status, and more.
</p>

<ul style="margin-left: 20px;">
  <li><b>ProvisioningState:</b> Succeeded</li>
  <li><b>Location:</b> westus2</li>
  <li><b>Replication:</b> Standard_RAGRS</li>
  <li><b>Resource Group:</b> msdocs-rg-[unique_id]</li>
</ul>

<p align="left">
  <img src="https://imgur.com/vdmaNQR.png" alt="Azure CLI output showing storage account list in table format" width="80%" />
</p>
<hr />

<h3>🧹 Post-Project Cleanup</h3>

<p align="left">
After completing the project, it's important to remove unused resources to avoid unnecessary charges. Since all project resources were created inside a dynamically named resource group, deleting that group also deletes the associated storage account and any other deployed services.
</p>

<pre>
<code>
az group delete --name &lt;your-resource-group-name&gt; --yes --no-wait
</code>
</pre>

<p align="left">
This command deletes the specified resource group and all its contents. The <code>--yes</code> flag skips the confirmation prompt, and <code>--no-wait</code> allows the deletion to process in the background so you can continue working or log out.
</p>

<p align="left">
For users who created multiple test resource groups, this loop can delete them all at once:
</p>

<pre>
<code>
for rg in $(az group list --query "[?starts_with(name, 'msdocs-rg-')].name" -o tsv); do
  echo "Deleting resource group: $rg"
  az group delete --name $rg --yes --no-wait
done
</code>
</pre>

<hr />

<h3>💵 Azure Cost Management Tips</h3>

<ul style="margin-left: 20px;">
  <li>Delete unused resource groups and services after testing to prevent ongoing charges.</li>
  <li>Use <code>az storage account list</code> and <code>az resource list</code> to audit active resources.</li>
  <li>Set up budgets and cost alerts in the Azure Portal under <b>Cost Management + Billing</b>.</li>
  <li>Prefer <b>locally-redundant storage (LRS)</b> for non-critical data to reduce costs compared to geo-redundant storage (RA-GRS).</li>
  <li>Use the <b>Azure Pricing Calculator</b> to estimate costs before deploying at scale.</li>
</ul>

<hr />

<h3>✅ Final Thoughts</h3>

<p align="left">
This project provided hands-on experience working with Azure CLI and Bash scripting to automate cloud infrastructure deployment. I gained insight into managing resources programmatically, understanding subscription contexts, and using best practices like dynamic naming and proper cleanup.
</p>

<p align="left">
Through each step — from verifying account details, deploying infrastructure, and validating results, to responsibly cleaning up — this exercise reinforced essential cloud operations skills applicable to real-world DevOps and cloud administration workflows.
</p>

<p align="left">
Thanks for checking out this walkthrough. Feel free to explore the scripts and reuse them in your own Azure automation workflows!
</p>
<hr />

<h3>✅ Tasks Completed</h3>

<ul style="list-style-type: none; padding-left: 0; margin-left: 0;">
  <li>☑️ Verified active Azure subscription and available regions</li>
  <li>☑️ Created a resource group using dynamic Bash variables</li>
  <li>☑️ Provisioned a geo-redundant StorageV2 account</li>
  <li>☑️ Validated storage account deployment with <code>az storage account list</code></li>
  <li>☑️ Executed post-project cleanup to remove resources</li>
</ul>

<hr />

<h3>📘 Try It Yourself</h3>

<p align="left">
Want to test this project in your own Azure environment? You can copy the CLI snippets above or run them directly in the <a href="https://shell.azure.com" target="_blank"><b>Azure Cloud Shell</b></a> — no local setup needed.
</p>

<p align="left">
Make sure you're signed in with an active Azure subscription and have permission to create and delete resource groups. Follow the steps in this
