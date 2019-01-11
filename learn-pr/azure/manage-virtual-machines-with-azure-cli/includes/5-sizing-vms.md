Velikost virtuálních počítačů je potřeba správně určit podle očekávané pracovní zátěže. Pokud virtuální počítač nemá dostatečně velkou paměť nebo procesor, selže při zatížení nebo poběží tak pomalu, že to nebude efektivní. 

## <a name="pre-defined-vm-sizes"></a>Předdefinované velikosti virtuálních počítačů

Při vytváření virtuálního počítače zadáváte hodnotu _Velikost virtuálního počítače_, která určuje, jak velké výpočetní prostředky bude mít virtuální počítač vyhrazené. K výpočetním prostředkům patří procesor, grafický procesor a paměť, které má virtuální počítač Azure k dispozici.

Podle očekávaného použití má Azure definovanou sadu různě velkých virtuálních počítačů pro Linux a Windows, ze kterých si můžete vybrat. 

| Typ | Velikosti | Popis |
|------|-------|-------------|
| Obecné účely   | Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7 | Vyvážený poměr procesorů k paměti. Ideální pro vývoj nebo testování a pro malé až střední řešení aplikací a dat. |
| Optimalizované pro výpočty. | Fs, F | Vysoký poměr procesorů k paměti. Vhodné pro aplikace se středním provozem, síťová zařízení a dávkové procesy. |
| Optimalizované pro paměť.  | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Vysoký poměr paměti k jádrům. Velmi vhodné pro relační databáze, střední a velké mezipaměti a analýzu v paměti. |
| Optimalizované pro úložiště. | Ls | Vysoká propustnost disku a V/V. Ideální pro databáze NoSQL, SQL a velké objemy dat. |
| Optimalizované z hlediska GPU. | NV, NC | Specializované virtuální počítače určené pro náročné vykreslování grafiky a úpravy videa. |
| Vysoký výkon | H, A8-11 | Naše procesorově nejvýkonnější virtuální počítače s volitelnými síťovými rozhraními s vysokou propustností (RDMA). | 

Dostupné velikosti se mění v závislosti na oblasti, ve které virtuální počítač vytváříte. Seznam dostupných velikostí můžete získat pomocí příkazu `vm list-sizes`. Zkuste do služby Azure Cloud Shell zadat následující:

```azurecli
az vm list-sizes --location eastus --output table
```

Zde je zkrácená odpověď na příkaz `eastus`:

```
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          2048  Standard_B1ms                         1           1047552                    4096
                 2          1024  Standard_B1s                          1           1047552                    2048
                 4          8192  Standard_B2ms                         2           1047552                   16384
                 4          4096  Standard_B2s                          2           1047552                    8192
                 8         16384  Standard_B4ms                         4           1047552                   32768
                16         32768  Standard_B8ms                         8           1047552                   65536
                 4          3584  Standard_DS1_v2                       1           1047552                    7168
                 8          7168  Standard_DS2_v2                       2           1047552                   14336
                16         14336  Standard_DS3_v2                       4           1047552                   28672
                32         28672  Standard_DS4_v2                       8           1047552                   57344
                64         57344  Standard_DS5_v2                      16           1047552                  114688
        ....
                64       3891200  Standard_M128-32ms                  128           1047552                 4096000
                64       3891200  Standard_M128-64ms                  128           1047552                 4096000
                64       3891200  Standard_M128ms                     128           1047552                 4096000
                64       2048000  Standard_M128s                      128           1047552                 4096000
                64       1024000  Standard_M64                         64           1047552                 8192000
                64       1792000  Standard_M64m                        64           1047552                 8192000
                64       2048000  Standard_M128                       128           1047552                16384000
                64       3891200  Standard_M128m                      128           1047552                16384000
```

## <a name="specifying-a-size-during-vm-creation"></a>Určení velikosti vytvářeného virtuálního počítače

Při vytváření našeho virtuálního počítače jsme velikost nezadali, takže služba Azure nám vybrala výchozí velikost pro obecné účely `Standard_DS1_v2`. Velikost jinak můžeme zadat pomocí parametru `--size`, který je součástí příkazu `vm create`. Pomocí následujícího příkazu byste například mohli vytvořit virtuální počítač se 16 jádry:

```azurecli
az vm create --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM2 \
  --image Debian --admin-username aldis --generate-ssh-keys --verbose \
  --size "Standard_DS5_v2"
```

> [!WARNING]
> Vaše úroveň předplatného [určuje omezení](https://docs.microsoft.com/azure/azure-subscription-service-limits) počtu prostředků, které můžete vytvořit, i jejich celkové velikosti. U předplatného s průběžnými platbami jste omezeni na **20 virtuálních procesorů** a u úrovně Free pouze na **4 virtuální procesory**. Pokud toto omezení překročíte, oznámí Azure CLI chybu **překročení kvóty**. Pokud se s touto chybou setkáte ve vlastním placeném předplatném, můžete si [zdarma podat online žádost](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-quota-errors) o zvýšení limitů spojených s vaším placeným předplatným (až na 10 000 vCPU).

## <a name="resizing-an-existing-vm"></a>Změna velikosti stávajícího virtuálního počítače
Velikost stávajícího virtuálního počítače můžeme také změnit, když se změní jeho zatížení nebo pokud jste při vytváření nastavili špatnou velikost. Před odesláním požadavku na změnu velikosti ale musíme zkontrolovat, jestli je požadovaná velikost dostupná v clusteru, do kterého náš virtuální počítač patří. Můžeme to provést pomocí příkazu `vm list-vm-resize-options`:

```azurecli
az vm list-vm-resize-options --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --output table
```

Ten nám vrátí seznam všech konfigurací velikostí, které jsou ve skupině prostředků dostupné. Pokud požadovaná velikost není v našem clusteru k dispozici, ale _je_ k dispozici v dané oblasti, můžeme [zrušit přidělení virtuálního počítače](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-deallocate). Tento příkaz spuštěný virtuální počítač zastaví a odebere ho z aktuálního clusteru bez ztráty prostředků. Potom můžeme změnit jeho velikost. To znamená, že virtuální počítač bude znovu vytvořen v novém clusteru, kde je k dispozici nakonfigurovaná velikost.

> [!NOTE]
> Azure Sandbox je omezený na několik velikostí virtuálních počítačů. V bezplatném předplatném Microsoft Learn je většina možností zakázaná.

Ke změně velikosti virtuálního počítače použijte příkaz `vm resize`. Můžete třeba zjistit, že pro vykonávané úlohy je virtuální počítač poddimenzovaný. Můžete ho posunout o několik úrovní až na DS3_v2, kde bude mít 4 virtuální jádra a 14 GB paměti. V Cloud Shellu zadejte následující příkaz:

```azurecli
az vm resize --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM --size Standard_DS3_v2
```

Než tento příkaz omezí prostředky virtuálního počítače, bude to několik minut trvat. Po dokončení vrátí novou konfiguraci JSON.