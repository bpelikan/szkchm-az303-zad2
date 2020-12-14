# szkchm-az303-zad2

![ARM template deploy](https://github.com/bpelikan/szkchm-az303-zad2/workflows/ARM%20template%20deploy/badge.svg)


## GitHub Actions configuration
```bash
# settings -> secrets -> create secret AZURE_SUBSCRIPTION_ID with value from $SUBSCRIPTION_ID
SUBSCRIPTION_ID=$(az account show -o json | jq -r ".id")

# save json from output and save in secret AZURE_CREDENTIALS
az ad sp create-for-rbac --name "szkchm-az303-zad2" --role contributor \
                          --scopes /subscriptions/$SUBSCRIPTION_ID \
                          --sdk-auth

# resource group for deployment
RG_NAME="szkchmzad2"
LOCATION="westeurope"
az group create --name $RG_NAME --location $LOCATION

# create Key Vault for secrets
RG_NAME_VAULT="mykv"
VAULT_NAME="szkchmzad2-kv"
ADMIN_LOGIN="vmadminlogin$RANDOM"
ADMIN_PASS="AdminPassword$RANDOM"
az group create --name $RG_NAME_VAULT --location $LOCATION
az keyvault create --location $LOCATION --name $VAULT_NAME --resource-group $RG_NAME_VAULT
az keyvault update --name $VAULT_NAME --resource-group $RG_NAME_VAULT --enabled-for-template-deployment true

az keyvault secret set --name "vmAdminUsername" --vault-name $VAULT_NAME --value $ADMIN_LOGIN
az keyvault secret set --name "vmAdminPassword" --vault-name $VAULT_NAME --value $ADMIN_PASS
```