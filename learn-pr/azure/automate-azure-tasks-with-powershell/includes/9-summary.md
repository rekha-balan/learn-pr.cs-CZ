V tomto modulu jsme napsali skript na automatizaci vytváření několika virtuálních počítačů. Přestože je skript celkem krátký, můžete si u něj všimnout jeho potenciálu při kombinaci smyček, proměnných a funkcí z PowerShellu s rutinami z modulu Azure PowerShell.

Modul Azure PowerShell je dobrou automatizační volbou pro správce, který má zkušenosti s prostředím PowerShell. Kombinace čisté syntaxe a výkonného skriptovacího jazyka stojí za zvážení, i když s prostředím PowerShell zkušenosti nemáte. Tato úroveň automatizace pro časově náročné úlohy, které jsou náchylné na chyby, by vám měla pomoct zredukovat čas potřebný na správu a zvýšit kvalitu.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

Při využívání vašeho vlastního předplatného můžete pomocí následující rutiny PowerShellu odstranit skupinu prostředků (a všechny související prostředky).

```powershell
Remove-AzResourceGroup -Name MyResourceGroupName
```

Pokud budete vyzváni k potvrzení odstranění, odpovězte **Ano**, případně můžete přidat parametr `-Force` pro přeskočení této výzvy. Provedení tohoto příkazu může trvat i několik minut.