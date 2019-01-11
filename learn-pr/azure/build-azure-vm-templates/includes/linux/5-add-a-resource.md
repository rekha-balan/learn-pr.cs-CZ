Při vytváření virtuálního počítače pro finanční analytiky zpravidla zahrnete webový server, který analytikovi nabízí řídicí panel pro vizualizaci a shromáždění výsledků. Použití šablony Resource Manageru ke zřízení virtuálního počítače představuje dobrý začátek. Jak ale můžete šablonu rozšířit o konfiguraci softwaru webového serveru v tomto virtuálním počítači?

Pomocí Azure CLI můžete ve virtuálním počítači nakonfigurovat webový server jen s několika základními příkazy. Co se ale stane, když:

* Potřebujete virtuální počítače neustále vytvářet a odstraňovat?
* Vaše nasazení vyžadují další komponenty, například úložiště a sítě, což zvyšuje jejich složitost?

Správa jednotlivých prostředků prostřednictvím Azure CLI představuje dobrý začátek. Brzy ale zjistíte, že je to zdlouhavé a náchylné k chybám, protože pořád potřebujete prostředky správně vzájemně propojit. Potřebujete nějaké automatizovanější řešení.

Zde prověříte požadavky na webový server na virtuálním počítači a naučíte se sestavit prostředky, které můžete přidat do šablon.

> [!NOTE]
> Kód a příkazy, které vidíte na této stránce, jsou ilustrativní. Teď jen sledujte výklad. Zatím nemusíte upravovat žádné soubory ani spouštět žádné příkazy.

## <a name="whats-the-custom-script-extension"></a>Co je rozšíření vlastních skriptů?

Rozšíření vlastních skriptů představuje snadný způsob, jak stahovat a spouštět skripty na virtuálních počítačích Azure. Je to jen jeden z mnoha způsobů konfigurace virtuálního počítače po jeho zprovoznění.

Skripty můžete ukládat v úložišti Azure nebo ve veřejném umístění, jako je GitHub. Skripty můžete spouštět ručně nebo v rámci automatizovanějšího nasazení. Zde budete definovat prostředek, který brzy přidáte do šablony Resource Manageru. Tento prostředek použije rozšíření vlastních skriptů ke stažení skriptu Bash z GitHubu a jeho spuštění ve virtuálním počítači. Tento skript nainstaluje a nakonfiguruje softwarový balíček s webovým serverem Nginx a nakonfiguruje základní domovskou stránku.

## <a name="how-do-i-extend-a-resource-manager-template"></a>Jak můžu rozšířit šablonu Resource Manageru?

Vaše šablona Resource Manageru vytvoří virtuální počítač s Ubuntu Linuxem. Existuje několik způsobů, jak tuto šablonu rozšířit tak, aby se po spuštění virtuálního počítače nainstaloval Nginx.

Jedním z nich je vytvoření několika šablon, které vždy definují jen jednu část systému. Jejich _propojením_ nebo _vnořením_ pak můžete sestavit ucelenější systém. Během vytváření vlastních šablon si můžete sestavit knihovnu menších a detailnějších šablon a kombinovat je podle potřeby.

Jiný způsob spočívá v úpravě některé existující šablony tak, aby vyhovovala vašim potřebám. Tak budete postupovat v tomto modulu, protože často jde o nejrychlejší způsob, jak začít vytvářet vlastní šablony.

## <a name="build-the-template-resource"></a>Vytvoření prostředku šablony

Zde se dozvíte, jak vytvářet prostředky šablony s využitím rozšíření vlastních skriptů jako příkladu. Jako výchozí bod použijete referenční dokumentaci a následně definujete potřebné vlastnosti prostředku.

Začněme kontrolou požadavků.

### <a name="gather-requirements"></a>Shromáždění požadavků

Tady je stručný přehled, čeho chcete prostřednictvím své šablony dosáhnout.

1. Vytvořit virtuální počítač.
1. Otevřít port 80 skrze síťovou bránu firewall.
1. Nainstalovat a nakonfigurovat na virtuálním počítači software webového serveru.

Šablona Resource Manageru, kterou jste použili v předchozí části, už splňuje první dva požadavky. Pokud jde o ten třetí, začněme tím, že se podíváme na příkaz Azure CLI, jehož ručním spuštěním z příkazového řádku bychom nainstalovali a nakonfigurovali Nginx na virtuálním počítači pomocí rozšíření vlastních skriptů.

```azurecli
az vm extension set \
  --resource-group <rgn>[sandbox resource group name]</rgn> \
  --vm-name MyUbuntuVM \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'
```

Tento příkaz používá rozšíření vlastních skriptů ke spuštění skriptu Bash, který na virtuálním počítači nainstaluje Nginx. Pokud chcete, můžete si tento [skript Bash prostudovat](https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh?azure-portal=true) na samostatné kartě prohlížeče.

Potřebujete nějak zajistit, aby tento příkaz používal syntaxi šablony Resource Manageru. Výše uvedený příklad `az vm extension set` je příkaz, který byste zadali na příkazovém řádku. To, co potřebujete, je deklarativní JSON ve formátu šablony.

### <a name="locate-the-resource-schema"></a>Vyhledání schématu prostředku

Jednou z možností, jak se naučit používat rozšíření vlastních skriptů v šabloně, je učení pomocí příkladů. Můžete například najít šablonu Azure pro rychlý start, která dělá něco podobného, a přizpůsobit ji podle svých potřeb.

Další možnost spočívá ve vyhledání potřebného prostředku v [referenční dokumentaci](https://docs.microsoft.com/azure/templates?azure-portal=true). Když v tomto případě prohledáte dokumentaci, objevíte [Microsoft.Compute virtualMachines/extensions template reference](https://docs.microsoft.com/azure/templates/Microsoft.Compute/2018-10-01/virtualMachines/extensions?azure-portal=true).

Můžete začít tím, že toto schéma zkopírujete do nějakého dočasného dokumentu.

```json
{
  "name": "string",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-10-01",
  "location": "string",
  "tags": {},
  "properties": {
    "publisher": "string",
    "type": "string",
    "typeHandlerVersion": "string",
    "autoUpgradeMinorVersion": boolean,
    "settings": {},
    "protectedSettings": {},
    "instanceView": {
      "name": "string",
      "type": "string",
      "typeHandlerVersion": "string",
      "substatuses": [
        {
          "code": "string",
          "level": "string",
          "displayStatus": "string",
          "message": "string",
          "time": "string"
        }
      ],
      "statuses": [
        {
          "code": "string",
          "level": "string",
          "displayStatus": "string",
          "message": "string",
          "time": "string"
        }
      ]
    }
  }
}
```

### <a name="specify-required-properties"></a>Zadání povinných vlastností

Schéma zobrazuje všechny vlastnosti, které můžete zadat. Některé vlastnosti jsou povinné, jiné jsou nepovinné. Můžete začít identifikací všech povinných vlastností. Najdete je pod definicí schématu na referenční stránce.

Zde jsou povinné parametry.

* `name`
* `type`
* `apiVersion`
* `location`
* `properties`

Po odebrání všech parametrů, které nejsou povinné, může definice prostředku vypadat nějak takto.

```json
{
  "name": "[concat(variables('vmName'), '/', 'ConfigureNginx')]",
  "type": "Microsoft.Azure.Extensions/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": { }
}
```

Hodnoty vlastností `type` a `apiVersion` pocházejí přímo z dokumentace. Vlastnost `properties` je povinná, ale prozatím může být prázdná.

Víte, že vaše existující šablona virtuálního počítače obsahuje parametr s názvem `location`. V tomto příkladu se k načtení této hodnoty používá předdefinovaná funkce `parameters`.

Vlastnost `name` dodržuje speciální konvenci. V tomto příkladu se používá předdefinovaná funkce `concat` ke zřetězení (zkombinování) několika řetězců. Úplný řetězec obsahuje název virtuálního počítače, za nímž následuje lomítko `/` a zvolený název (v tomto příkladu „ConfigureNginx“). Název virtuálního počítače pomáhá šabloně identifikovat, na kterém prostředku virtuálního počítače se má skript spustit.

### <a name="specify-additional-properties"></a>Zadání dalších vlastností

V dalším kroku můžete přidat jakékoli další potřebné vlastnosti. Když se znovu podíváte na předchozí příklad, potřebujete umístění (neboli identifikátor URI) souboru skriptu a příkaz Bash, který na virtuálním počítači spustí tento skript. Doporučuje se zahrnout rovněž název vydavatele, typ a verzi prostředku.

Na základě dokumentace potřebujete tyto hodnoty, které jsou zde znázorněné pomocí „tečkového“ zápisu.

* `properties.publisher`
* `properties.type`
* `properties.typeHandlerVersion`
* `properties.autoUpgradeMinorVersion`
* `properties.settings.fileUris`
* `properties.protectedSettings.commandToExecute`

Prostředek Rozšíření vlastních skriptů teď vypadá nějak takto.

```json
{
  "name": "[concat(variables('vmName'), '/', 'ConfigureNginx')]",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "customScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "./configure-nginx.sh"
    }
  }
}
```

### <a name="specify-dependent-resources"></a>Zadání závislých prostředků

Rozšíření vlastních skriptů nelze spustit, dokud není dostupný virtuální počítač. Všechny prostředky šablony poskytují vlastnost `dependsOn`. Tato vlastnost pomáhá Resource Manageru určit správné pořadí, v jakém se mají prostředky používat.

Po přidání vlastnosti `dependsOn` může prostředek šablony vypadat takto.

```json
{
  "name": "[concat(variables('vmName'), '/', 'ConfigureNginx')]",
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "apiVersion": "2018-06-01",
  "location": "[parameters('location')]",
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "customScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "./configure-nginx.sh"
    }
  },
  "dependsOn": [
    "[resourceId('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
  ]
}
```

Syntaxe s hranatými závorkami `[ ]` znamená, že můžete zadat pole (seznam) prostředků, které musí před použitím tohoto prostředku existovat.

Závislost prostředku lze definovat několika způsoby. Můžete zadat jeho název, například „MyUbuntuVM“, jeho úplný název (včetně oboru názvů, typu a názvu), například „Microsoft.Compute/virtualMachines/MyUbuntuVM“, nebo ID prostředku.

V tomto příkladu se používá integrovaná funkce `resourceId` ke získání ID prostředku virtuálního počítače pomocí jeho úplného názvu. To umožňuje přesně určit, na jaký prostředek odkazujete, a pomáhá zamezit nejednoznačnosti, pokud mají některé prostředky podobný název.

Existující šablona obsahuje proměnnou `vmName`, která definuje název virtuálního počítače. V tomto příkladu se k jejímu načtení používá předdefinovaná funkce `variables`.

## <a name="summary"></a>Shrnutí

Teď máte všechno potřebné k tomu, abyste v šabloně mohli definovat prostředek Rozšíření vlastních skriptů. V další části šablonu rozšíříte tak, abyste ji mohli použít.
