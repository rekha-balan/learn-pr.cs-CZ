## <a name="get-an-access-key"></a>Získání přístupového klíče

Pokud jste tak ještě neučinili, spusťte v Azure Cloud Shell následující příkaz k uložení přístupového klíče rozhraní API do proměnné s názvem `key`. Tuto proměnnou použijeme v následných volání.

```azurecli
key=$(az cognitiveservices account keys list \
--name ComputerVisionService \
--resource-group <rgn>[sandbox resource group name]</rgn> \
--query key1 -o tsv)
```
