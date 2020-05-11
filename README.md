### Azure Front Door Custom Domain
Configuration and Common Issues


*You likely setup TSL/SSL for your Azure Front Door through the Azure Portal and that's fine. However, after doing so you probably left wonder what now.* 

*At the time of this writing, Azure CLI has the more verbose Azure Front Door commands.*

### So, you've enabled HTTPS on your Front Door with either an Azure managed certificate or stored your own in Azure Key Vault.

<pre>
az network front-door frontend-endpoint enable-https --front-door-name
                                                     --name
                                                     --resource-group
                                                     [--certificate-source {AzureKeyVault, FrontDoor}]
                                                     [--minimum-tls-version {1.0, 1.2}]
                                                     [--only-show-errors]
                                                     [--secret-name]
                                                     [--secret-version]
                                                     [--vault-id]
</pre>
*Well, before you start using Azure CLI to manage your Front Door, you'll need to add the extension.*

### Adds the front door extension
az extension add --name front-door

*The most common problem with using certificates is the Certificate Name Check. It's enabled by default and currently isn't accessible from the portal.*

### Check if enforce certificate name check is enabled 
az network front-door list | grep enforceCertificateNameCheck

### Disable certificate name check
az network front-door update --name "your front door's name" --resource-group "the Resource Group" --enforce-certificate-name-check Disabled

*How about just knowing if your CNAME record is correctly mapped to your Azure Front Door? You could open your web browser, paste your custom domain URL, then do the same for "your front door hostname".azurefd.net. If the same page is returned, you're good to go. Then again, there's an Azure CLI command for that too. I'm looking forward to see what it returns.*

### Check if your custom domain is mapping to your front door via DNS
az network front-door check-custom-domain --host-name "your frontend hostname" --resource-group "the resource group" --name "your front door's name"
