Azure CLI umožňuje zadávání příkazů a jejich okamžité spouštění z příkazového řádku. Připomínáme, že celkovým cílem příkladu vývoje softwaru je nasadit nová sestavení webové aplikace pro účely testování. Probereme druhy úloh, které můžete pomocí Azure CLI dělat.

## <a name="what-azure-resources-can-be-managed-using-the-azure-cli"></a>Které prostředky Azure můžete spravovat pomocí Azure CLI?

Azure CLI umožňuje řídit skoro všechny aspekty jednotlivých prostředků Azure. Můžete pracovat se skupinami prostředků, úložištěm, virtuálními počítači, Azure Active Directory (Azure AD), kontejnery, strojovým učením a podobně.

Příkazy jsou v rozhraní příkazového řádku strukturované ve _skupinách_ a _podskupinách_. Každá skupina představuje službu, kterou poskytuje Azure, a podskupiny rozdělují příkazy pro tyto služby do logických seskupení. Například skupina `storage` obsahuje mimo jiné podskupiny **account**, **blob**, **storage** a **queue**.

Jak tedy najdete konkrétní příkazy, které potřebujete? Můžete například použít `az find`. Pokud například chcete najít příkazy, které pomáhají se správou úložiště objektů blob, můžete použít následující příkaz find:

```azurecli
az find -q blob
```

Pokud název požadovaného příkazu už znáte, můžete pomocí argumentu `--help` zjistit podrobnější informace o tomto příkazu. U skupiny příkazů tímto způsobem zjistíte seznam dostupných podpříkazů. Takto můžete v našem příkladu úložiště získat seznam podskupin a příkazů pro správu úložiště objektů blob:

```azurecli
az storage blob --help
```

## <a name="how-to-create-an-azure-resource"></a>Jak vytvořit prostředek Azure

Vytváření nového prostředku Azure má obvykle tři kroky: připojení k předplatnému Azure, vytvoření prostředku a ověření, že vytvoření proběhlo úspěšně. Následující obrázek znázorňuje přehled tohoto procesu.

![Obrázek znázorňující postup vytvoření prostředku Azure pomocí rozhraní příkazového řádku](../media/4-create-resources-overview.png)

Každý krok odpovídá jinému příkazu Azure CLI.

### <a name="connect"></a>Připojení

Vzhledem k tomu, že pracujete s místní instalací Azure CLI, budete se před spouštěním příkazů Azure muset ověřit pomocí příkazu **login** v Azure CLI.

```azurecli
az login
```

Azure CLI zpravidla při otevření přihlašovací stránky Azure spustí váš výchozí prohlížeč. Pokud to nefunguje, postupujte podle pokynů příkazového řádku a zadejte do [https://aka.ms/devicelogin](https://aka.ms/devicelogin) autorizační kód.

Po úspěšném přihlášení budete připojeni k vašemu předplatnému Azure.

### <a name="create"></a>Vytvoření

Často potřebujete vytvořit novou skupinu prostředků dřív, než vytvoříte novou službu Azure. Proto jako příklad toho, jak vytvářet prostředky Azure z rozhraní příkazového řádku, použijeme právě skupinu prostředků.

Skupinu prostředků vytvoříte v Azure CLI pomocí příkazu **group create**. Je potřeba zadat název a umístění. Název musí být v rámci předplatného jedinečný. Umístění určuje, kam se uloží metadata pro vaši skupinu prostředků. K určení umístění použijete řetězce jako Západní USA, Severní Evropa nebo Západní Indie. Další možností je použít jejich jednoslovné ekvivalenty (například westus, northeurope nebo westindia). Základní syntaxe:

```azurecli
az group create --name <name> --location <location>
```

> [!IMPORTANT]
> Při použití bezplatné služby Azure Sandbox nemusíte skupinu prostředků vytvářet. Místo toho použijete předem vytvořenou skupinu prostředků.

### <a name="verify"></a>Ověření

Azure CLI poskytuje pro celou řadu prostředků podpříkaz **list**, který umožňuje zobrazit jejich podrobnosti. Například příkaz **group list** Azure CLI zobrazí seznam skupin prostředků Azure. To nám teď umožní ověřit, jestli se skupina prostředků úspěšně vytvořila:

```azurecli
az group list
```

Pokud chcete získat přehlednější zobrazení, můžete výstup formátovat jako jednoduchou tabulku:

```azurecli
az group list --output table
```
