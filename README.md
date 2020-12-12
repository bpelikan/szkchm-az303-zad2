# szkchm-az303-zad2

![ARM template deploy](https://github.com/bpelikan/szkchm-az303-zad2/workflows/ARM%20template%20deploy/badge.svg)


## GGitHub Actions configuration
```bash
# repo settings -> secrets -> create secrets AZURE_SUBSCRIPTION_ID with value from `echo $SUBSCRIPTION_ID`
SUBSCRIPTION_ID=$(az account show -o json | jq -r ".id")

# save json from output and save in repo secret AZURE_CREDENTIALS
az ad sp create-for-rbac --name "szkchm-az303-zad2" --role contributor \
                          --scopes /subscriptions/$SUBSCRIPTION_ID \
                          --sdk-auth

# rg for template
RG_NAME="szkchmzad2"
LOCATION="westeurope"
az group create --name $RG_NAME --location $LOCATION
```