Webový server funguje, ale zjistili jste, že potřebujete větší výpočetní výkon, aby byli uživatelé spokojeni. Jak můžete zajistit, aby váš virtuální počítač běžel rychleji?

Ve svém datovém centru můžete problémy s výkonem vyřešit přesunutím webového serveru na výkonnější hardware. Problém je, že musíte nový systém koupit, zařadit do racku a zprovoznit. S Azure je odpověď mnohem jednodušší.

Než kapacitu virtuálního počítače vertikálně navýšíme (pro zvýšení výkonu), definujme si, co škálování znamená.

## <a name="what-is-scale"></a>Co je škálování?

Výrazem _škálování_ označujeme přidání šířky pásma sítě, paměti, úložiště nebo výpočetního výkonu pro dosažení lepšího výkonu.  

Možná jste zaslechli pojmy _vertikální_ a _horizontální navýšení kapacity_.

Vertikální navýšení kapacity znamená, že navyšujete paměť, úložiště nebo výpočetní výkon u existujícího virtuálního počítače. Můžete například přidat další paměť na webový nebo databázový server, aby běžel rychleji.

Horizontální navýšení kapacity označuje doplnění dalších virtuálních počítačů pro zajištění vaší aplikace. Můžete například vytvořit velký počet stejně nakonfigurovaných virtuálních počítačů a rozdělovat mezi nimi práci pomocí nástroje pro vyrovnávání zatížení.

> [!TIP]
> Cloud je flexibilní. Pokud potřebujete vertikálně nebo horizontálně navýšit kapacitu jenom dočasně, můžete potom kapacitu pro vaše nasazení zase _vertikálně_ nebo _horizontálně snížit_. Vertikální nebo horizontální snížení kapacity vám pomůže ušetřit.<br><br>K optimalizaci útraty v cloudu můžete využít dvě služby, **Azure Advisor** a **Azure Cost Management**. Tyto služby vám pomohou zjistit, kde využíváte víc, než je potřeba, a potom kapacitu snížit tak, aby odpovídala skutečnému využití.

## <a name="scale-up-your-vm"></a>Vertikální navýšení kapacity virtuálního počítače

Jak si určitě vzpomínáte, při vytváření virtuálního počítače jste zadali velikost **Standard_DS2_v2**. Váš virtuální počítač má teď 2 virtuální procesory a 7 GB paměti.

Zvyšme velikost o stupeň výš – na **Standard_DS3_v2**. Váš virtuální počítač pak bude mít 4 virtuální procesory a 14 GB paměti.

::: zone pivot="windows-cloud"

1. Ze služby Cloud Shell spusťte `az vm resize` pro zvýšení velikosti virtuálního počítače na **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    Tento aktualizační proces trvá přibližně minutu. Během něho se virtuální počítač restartuje.

1. Spuštěním `az vm show` ověřte, že virtuální počítač používá novou velikost.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Uvidíte novou velikost virtuálního počítače: **Standard_DS3_v2**.
    ```output
    Standard_DS3_v2
    ```

::: zone-end

::: zone pivot="linux-cloud"

1. Ze služby Cloud Shell spusťte `az vm resize` pro zvýšení velikosti virtuálního počítače na **Standard_DS3_v2**.

    ```azurecli
    az vm resize \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --size Standard_DS3_v2
    ```
    Tento aktualizační proces trvá přibližně minutu. Během něho se virtuální počítač restartuje.

1. Spuštěním `az vm show` ověřte, že virtuální počítač používá novou velikost.

    ```azurecli
    az vm show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name myVM \
      --query "hardwareProfile" \
      --output tsv
    ```
    Uvidíte novou velikost virtuálního počítače: **Standard_DS3_v2**.
    ```output
    Standard_DS3_v2
    ```

::: zone-end

## <a name="summary"></a>Shrnutí

Skvělá práce! Stačil jeden příkaz a virtuální počítač má teď dvojnásobný výkon.

Vertikální navýšení kapacity a horizontální navýšení kapacity představuje dva způsoby, jak zvýšit výkon. Vertikálně jste navýšili kapacitu virtuálního počítače, aby se zvýšil jeho výpočetní výkon.
