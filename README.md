# szkchm-az303-zad2


```bash
SUBSCRIPTION_ID=""
az ad sp create-for-rbac --name "szkchm-az303-zad2" --role contributor \
                          --scopes /subscriptions/$SUBSCRIPTION_ID \
                          --sdk-auth


```