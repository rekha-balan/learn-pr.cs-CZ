Pokud už Azure nějakou dobu používáte, určitě jste slyšeli o Azure Resource Manageru. Pojďme se na roli Resource Manageru podívat a definovat, co tvoří šablonu Resource Manageru.

## <a name="whats-azure-resource-manager"></a>Co je Azure Resource Manager?

Azure Resource Manager je rozhraní pro správu a organizaci cloudových prostředků. Představte si Resource Manager jako způsob nasazení cloudových prostředků.

Pokud znáte skupiny prostředků Azure, víte, že vám umožňují pracovat se sadami souvisejících prostředků jako s celkem. Resource Manager slouží k organizaci skupin prostředků, které vám umožňují nasazovat, spravovat a odstraňovat všechny prostředky najednou v rámci jedné akce.

Přemýšlejte o finančních modelech, které provozujete pro analytiky. K provozování modelu potřebujete jeden nebo více virtuálních počítačů, databázi k ukládání dat a virtuální síť, která umožňuje připojení ke všemu potřebnému. Pomocí Resource Manageru nasadíte tyto prostředky do stejné skupiny prostředků a následně je spravujete a monitorujete společně. Až skončíte, můžete všechny prostředky v této skupině prostředků odstranit jedinou operací.

### <a name="what-are-resource-manager-templates"></a>Co jsou šablony Resource Manageru?

_Šablona_ Resource Manageru přesně definuje všechny prostředky Resource Manageru v určitém nasazení. Šablonu Resource Manageru můžete do skupiny prostředků nasadit jako jednu operaci.

Šablona Resource Manageru je soubor JSON, díky čemuž má formu _deklarativní automatizace_. Deklarativní automatizace znamená, že definujete, _jaké_ prostředky potřebujete, ale nikoli to, _jak_ se mají vytvořit. Jinými slovy definujete to, co potřebujete, a za správné nasazení příslušných prostředků zodpovídá Resource Manager.

Deklarativní automatizace se podobá tomu, jak webové prohlížeče zobrazují soubory HTML. Soubor HTML popisuje, _jaké_ elementy se objeví na stránce, ale nepopisuje, _jak_ se mají zobrazit. Za toto „jak“ zodpovídá webový prohlížeč.

> [!NOTE]
> Ostatní mohou šablony Resource Manageru označovat jako „šablony ARM“. Dáváme přednost úplným názvům „šablony Azure Resource Manageru“ nebo „šablony Resource Manageru“.

## <a name="why-use-resource-manager-templates"></a>Proč používat šablony Resource Manageru?

Díky používání šablon Resource Manageru budou vaše nasazení rychlejší a opakovatelnější. Nemusíte už například vytvořit virtuální počítač na portálu, počkat na jeho dokončení, pak vytvořit další atd. Resource Manager se postará o kompletní nasazení za vás.

Zde jsou některé další výhody, které stojí za zvážení.

* **Šablony zlepšují konzistenci**

    Šablony Resource Manageru poskytují vám a ostatním společný jazyk pro popis nasazení. Struktura, formát a výrazy uvnitř šablony zůstávají stejné bez ohledu na to, jaký nástroj nebo sada SDK se používá k nasazení této šablony.

* **Šablony pomáhají vyjádřit složitá nasazení**

    Šablony umožňují nasadit několik prostředků ve správném pořadí. Virtuální počítač byste například nechtěli nasadit před vytvořením disku s operačním systémem nebo síťového rozhraní. Resource Manager má přehled o všech prostředcích a jejich závislých prostředcích a nejdříve vytvoří závislé prostředky. Mapování závislostí pomáhá zajistit, aby se nasazení prováděla ve správném pořadí.

* **Šablony omezují ruční úlohy náchylné k chybám**

    Ruční vytváření a propojování prostředků může být časově náročné a během tohoto procesu mohou snadno vzniknout chyby. Resource Manager zajišťuje, že nasazení proběhne pokaždé stejný způsobem.

* **Šablony jsou vlastně kód**

    Šablony vyjadřují vaše požadavky prostřednictvím kódu. Šablonu si můžete představit jako typ _infrastruktury jako kódu_, která se dá sdílet, testovat a udržovat pomocí verzí jako jakýkoli jiný software. Protože šablony jsou kód, můžete si navíc vytvořit „psaný záznam“, který se dá sledovat. Kód šablony slouží k dokumentaci nasazení. Většina uživatelů uchovává šablony v určitém systému pro správu revizí, jako je Git. Při změnách šablony také její historie revizí dokumentuje, jak se šablona (a vaše nasazení) průběžně vyvíjí.

* **Šablony podněcují opakované použití**

    Šablona může obsahovat parametry, které se vyplní při spuštění šablony. Parametr může definovat uživatelské jméno nebo heslo, název domény atd. Pomocí parametrů šablony můžete vytvořit více verzí infrastruktury, například přípravnou a produkční, ale pořád k tomu využívat stejnou šablonu.

* **Šablony se dají propojovat**

    Šablony Resource Manageru lze vzájemně propojovat, díky čemuž jsou modulární. Můžete vytvořit malé šablony, které definují jednotlivé části řešení, a jejich kombinací pak vytvořit kompletní systém.

Modely, které používají finanční analytici, jsou jedinečné, ale v základní infrastruktuře si můžete všimnout určitých vzorů. Většina modelů například vyžaduje databázi k ukládání dat. Mnohé modely používají stejné programovací jazyky, architektury a operační systémy. Můžete definovat šablony, které popisují každou jednotlivou součást (výpočetní prostředky, úložiště, sítě atd.), a jejich kombinací splnit potřeby různých analytiků.

## <a name="whats-in-a-resource-manager-template"></a>Co obsahuje šablona Resource Manageru?

> [!NOTE]
> Zde uvidíte pár příkladů kódu, abyste získali představu o struktuře jednotlivých oddílů. Nemějte obavy, pokud se v tom ztrácíte – jakmile si šablony trochu osaháte, porozumíte šablonám jiných uživatelů a vytvoříte si své vlastní.

Možná k odesílání dat mezi servery a webovými aplikacemi používáte JSON nebo JavaScript Object Notation. JSON je rovněž oblíbeným způsobem popisu konfigurace aplikací a infrastruktury. 

JSON umožňuje vyjádřit data uložená jako objekt (například virtuální počítač) v textové podobě. Dokument JSON je v podstatě kolekce dvojic klíč-hodnota. Každý klíč je řetězec. Jeho hodnota může být řetězec, číslo, logický výraz, seznam hodnot nebo objekt (což je kolekce jiných dvojic klíč-hodnota).

Šablona Resource Manageru může obsahovat následující oddíly. Tyto oddíly jsou vyjádřeny pomocí notace JSON, ale nesouvisí se samotným jazykem JSON.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "functions": [  ],
    "resources": [  ],
    "outputs": {  }
}
```
Podívejme se na každý z těchto oddílů trochu podrobněji.

### <a name="parameters"></a>Parametry

Zde se zadává, které hodnoty se dají konfigurovat při spuštění šablony. Uživatelům šablony můžete například umožnit zadání uživatelského jména, hesla nebo názvu domény.

Zde je příklad, který ukazuje dva parametry &ndash; jeden pro uživatelské jméno virtuálního počítače a jeden pro jeho heslo.

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "Username for the Virtual Machine."
    }
  },
  "adminPassword": {
    "type": "securestring",
    "metadata": {
      "description": "Password for the Virtual Machine."
    }
  }
}
```

### <a name="variables"></a>Proměnné 

Zde se definují hodnoty, které se v šabloně používají. Proměnné umožňují snadnější správu šablon. Můžete například jednou definovat název účtu úložiště jako proměnnou a používat ji na jiných místech šablony. Pokud se název účtu úložiště změní, stačí aktualizovat jen tuto proměnnou.

Zde je příklad, který ukazuje několik proměnných popisujících síťové funkce virtuálního počítače.

```json
"variables": {
  "nicName": "myVMNic",
  "addressPrefix": "10.0.0.0/16",
  "subnetName": "Subnet",
  "subnetPrefix": "10.0.0.0/24",
  "publicIPAddressName": "myPublicIP",
  "virtualNetworkName": "MyVNET"
}
```

### <a name="functions"></a>Funkce

Zde se definují procedury, které nechcete v rámci šablony opakovat. Stejně jako proměnné pomáhají funkce s jednodušší údržbou šablon. Tento příklad vytvoří funkci pro sestavení jedinečného názvu, který se dá použít při vytváření prostředků, které mají požadavky na globálně jedinečné pojmenování.

```json
"functions": [
  {
    "namespace": "contoso",
    "members": {
      "uniqueName": {
        "parameters": [
          {
            "name": "namePrefix",
            "type": "string"
          }
        ],
        "output": {
          "type": "string",
          "value": "[concat(toLower(parameters('namePrefix')), uniqueString(resourceGroup().id))]"
        }
      }
    }
  }
],
```

### <a name="resources"></a>Prostředky

V tomto oddílu se definují prostředky Azure, které jsou součástí nasazení.

Zde je příklad, který vytvoří prostředek veřejné IP adresy.

```json
{
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIPAddressName')]",
  "location": "[parameters('location')]",
  "apiVersion": "2018-08-01",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsLabelPrefix')]"
    }
  }
}
```

Typ tohoto prostředku je `Microsoft.Network/publicIPAddresses`. Jeho název se načte z oddílu proměnných a jeho umístění (neboli oblast Azure) se načte z oddílu parametrů.

Protože typy prostředků se v průběhu času mohou měnit, odkazuje `apiVersion` na verzi typu prostředku, který chcete použít. S tím, jak se typy prostředků vyvíjejí a mění, můžete šablony upravit tak, aby fungovaly s nejnovějšími funkcemi, až budete připraveni.

### <a name="outputs"></a>Výstupy

Zde se definují jakékoli informace, které chcete přijmout při spuštění šablony. Můžete například zobrazit IP adresu nebo plně kvalifikovaný název domény virtuálního počítače, což jsou údaje, které do spuštění nasazení neznáte.

Zde je příklad, který ukazuje výstup s názvem „hostname“. Hodnota plně kvalifikovaného názvu domény se načte z nastavení veřejné IP adresy virtuálního počítače.

```json
"outputs": {
  "hostname": {
    "type": "string",
    "value": "[reference(variables('publicIPAddressName')).dnsSettings.fqdn]"
  }
}
```

## <a name="how-do-i-write-a-resource-manager-template"></a>Jak si mohu vytvořit šablonu Resource Manageru?

Šablony Resource Manageru můžete vytvářet mnoha způsoby. I když si šablonu můžete napsat od začátku, je běžné začít s existující šablonou a upravit ji tak, aby odpovídala vašim potřebám.

Zde je několik způsobů, jak získáte šablonu do začátku:

* Pomocí webu Azure Portal vytvořte šablonu založenou na prostředcích v existující skupině prostředků.
* Začněte šablonou, kterou jste vytvořili vy nebo váš tým a která slouží k podobnému účelu.
* Začněte šablonou Azure pro rychlý start. V další části zjistíte, jak na to.

Bez ohledu na použitý způsob se k vytváření šablony používá textový editor. Můžete využít svůj oblíbený editor, ale Visual Studio Code s [rozšířením Nástroje Azure Resource Manageru](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools&azure-portal=true) je speciálně navržené pro vytváření šablon. Toto rozšíření usnadňuje navigaci v kódu šablony a poskytuje automatické dokončování pro mnoho běžných úloh.

Při zkoumání a vytváření šablon budete chtít [nahlížet do dokumentace](https://docs.microsoft.com/azure/templates?azure-portal=true), abyste porozuměli tomu, jaké typy prostředků jsou k dispozici a jak se používají.