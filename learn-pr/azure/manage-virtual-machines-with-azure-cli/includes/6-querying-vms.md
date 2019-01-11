Teď, když jsme vytvořili virtuální počítač, můžeme o něm získat informace prostřednictvím dalších příkazů.

Začneme s příkazem `vm list`.

```azurecli
az vm list
```

Tento příkaz vrátí _všechny_ virtuální počítače definované v tomto předplatném. Výstup je možné filtrovat podle konkrétní skupiny prostředků pomocí parametru `--resource-group`. 

## <a name="output-types"></a>Typy výstupu
Všimněte si, že výchozí typ odpovědi pro všechny příkazy, které jsme dosud provedli, je JSON. To je výhodné pro skriptování, ale pro většinu lidí obtížnější ke čtení. Typ výstupu pro kteroukoli odpověď můžete změnit příznakem `--output`. Zkuste si vyzkoušet různé styly výstupu v prostředí Azure Cloud Shell na následujícím příkazu.

```azurecli
az vm list --output table
```

S příkazem `table` můžete zadat `json` (výchozí), `jsonc` (obarvený JSON) nebo `tsv` (hodnoty rozdělené do tabulky). Zkuste varianty příkazu s výše uvedenými parametry a prohlédněte si výsledek.

## <a name="getting-the-ip-address"></a>Získání IP adresy

Dalším užitečným příkazem je `vm list-ip-addresses`, který zobrazí seznam veřejných a privátních IP adres virtuálního počítače. Pokud se adresy změní nebo pokud jste si je nepoznamenali při vytváření, můžete je získat kdykoli.

```azurecli
az vm list-ip-addresses -n SampleVM -o table
```

Příkaz vrací:

```
VirtualMachine    PublicIPAddresses    PrivateIPAddresses
----------------  -------------------  --------------------
SampleVM          168.61.54.62         10.0.0.4
```

> [!TIP]
> Všimněte si, že používáme zkrácenou syntaxi příznaku `--output`, tj. `-o`. Většinu parametrů příkazů Azure CLI je možné zkrátit na jednu pomlčku a písmeno. Například `--name` můžete zkrátit na `-n` a `--resource-group` na `-g`. To je užitečné při psaní, ale ve skriptech doporučujeme pro přehlednost používat úplné názvy parametrů. Podrobnosti o jednotlivých příkazech najdete v dokumentaci.

## <a name="getting-vm-details"></a>Získávání podrobností o virtuálním počítači

Pomocí příkazu `vm show` můžeme získat podrobnější informace o konkrétním virtuálním počítači podle jeho názvu nebo ID.

```azurecli
az vm show --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM
```

Příkaz vrátí poměrně velký blok JSON s nejrůznějšími informacemi o virtuálním počítači, včetně připojených úložných zařízení, síťových rozhraní a všech ID objektů pro prostředky, ke kterým je virtuální počítač připojený. Opět můžeme změnit formát výstupu na tabulku, ale tím ztratíme skoro všechna zajímavá data. Místo toho můžeme přepnout na integrovaný dotazovací jazyk pro JSON nazvaný [JMESPath](http://jmespath.org/).

## <a name="adding-filters-to-queries-with-jmespath"></a>Přidávání filtrů k dotazům pomocí JMESPath

JMESPath je standardní dotazovací jazyk postavené na objektech JSON. Nejjednodušším dotazem je zadání _identifikátoru_, který vybere v objektu JSON určitý klíč.

Mějme například objekt:

```json
{
  "people": [
    {
      "name": "Fred",
      "age": 28
    },
    {
      "name": "Barney",
      "age": 25
    },
    {
      "name": "Wilma",
      "age": 27
    }
  ]
}
```

Můžeme použít dotaz `people`, který vrátí pole hodnot pro pole `people`. Pokud budeme chtít jen _jednoho_ člověka, můžeme použít indexer. Například `people[1]` vrátí:

```json
{
    "name": "Barney",
    "age": 25
}
```

Můžeme také přidat konkrétní kvalifikátory, které vrátí podmnožinu objektů na základě určitých kritérií. Například přidaný kvalifikátor `people[?age > '25']` vrátí:

```json
[
  {
    "name": "Fred",
    "age": 28
  },
  {
    "name": "Wilma",
    "age": 27
  }
]
```

Nakonec můžeme výsledky omezit přidáním výběru `people[?age > '25'].[name]`, který vrátí jenom názvy:

```json
[
  [
    "Fred"
  ],
  [
    "Wilma"
  ]
]
```

JMESQuery má několik dalších zajímavých funkcí dotazování. Pokud máte čas, přečtěte si [online kurz](http://jmespath.org/tutorial.html), který je k dispozici webu [JMESPath.org](http://jmespath.org/).

## <a name="filtering-our-azure-cli-queries"></a>Filtrování dotazů v Azure CLI

Se základní znalostí o dotazech JMES můžeme na data vracená příkazy jako `vm show` použít filtry. Například můžeme načíst uživatelské jméno správce:

```azurecli
az vm show --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --query "osProfile.adminUsername"
```

Můžeme získat velikost přiřazenou k virtuálnímu počítači:

```azurecli
az vm show --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --query hardwareProfile.vmSize
```

Nebo když chcete načíst všechny identifikátory síťových rozhraní, můžete použít dotaz:

```azurecli
az vm show --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --query "networkProfile.networkInterfaces[].id"
```

Tato dotazovací technika bude fungovat se všemi příkazy Azure CLI a slouží k získání konkrétních dat na příkazovém řádku. Je užitečná také při skriptování – můžete například získat hodnotu ze svého účtu Azure a uložit ji v proměnné prostředí nebo skriptu. Když se rozhodnete pracovat tímto způsobem, užitečným příznakem je parametr `--output tsv` (který můžete zkrátit na `-o tsv`). Vrátí hodnoty oddělené tabulátorem, které obsahují jenom skutečná data.

Například

```azurecli
az vm show --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --query "networkProfile.networkInterfaces[].id" -o tsv
```

vrátí tento text: `/subscriptions/20f4b944-fc7a-4d38-b02c-900c8223c3a0/resourceGroups/2568d0d0-efe3-4d04-a08f-df7f009f822a/providers/Microsoft.Network/networkInterfaces/SampleVMVMNic`
