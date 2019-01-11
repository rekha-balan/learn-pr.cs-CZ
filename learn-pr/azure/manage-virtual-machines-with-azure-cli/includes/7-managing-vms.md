Hlavními úkoly při provozu virtuálních počítačů je jejich spouštění a zastavování.

## <a name="stopping-a-vm"></a>Zastavení virtuálního počítače

Běžící virtuální počítač můžeme zastavit pomocí příkazu `vm stop`. Musíte předat název a skupinu prostředků nebo jedinečné ID virtuálního počítače:

```azurecli
az vm stop -n SampleVM -g <rgn>[sandbox resource group name]</rgn>
```

Zastavení můžeme ověřit pokusem o odeslání příkazu ping na veřejnou IP adresu pomocí `ssh` nebo pomocí příkazu `vm get-instance-view`. Tento poslední přístup vrátí stejná základní data jako `vm show`, obsahuje ale podrobnosti o samotné instanci. Zkuste do Azure Cloud Shellu zadat následující příkaz, abyste zobrazili aktuální stav virtuálního počítače:

```azurecli
az vm get-instance-view -n SampleVM -g <rgn>[sandbox resource group name]</rgn> --query "instanceView.statuses[?starts_with(code, 'PowerState/')].displayStatus" -o tsv
```

Tento příkaz by měl jako výsledek vrátit `VM stopped`.

## <a name="starting-a-vm"></a>Spuštění virtuálního počítače

Obrácenou akci můžeme udělat pomocí příkazu `vm start`.

```azurecli
az vm start -n SampleVM -g <rgn>[sandbox resource group name]</rgn>
```

Tento příkaz spustí zastavený virtuální počítač. Ověřit to můžeme dotazem `vm get-instance-view`, který by měl vrátit `VM running`.

## <a name="restarting-a-vm"></a>Restartování virtuálního počítače

Pokud jsme udělali změny, které vyžadují restartování, můžeme virtuální počítač restartovat pomocí příkazu `vm restart`. Jestli chcete, aby se okamžitě vrátilo Azure CLI (bez čekání na restartování virtuálního počítače), můžete přidat příznak `--no-wait`.

