Oddělení IT docela často spravují velké sady prostředků Azure, které zahrnují virtuální počítače Azure nebo spravované weby.

Použití webu Azure Portal je jednoduché u jednorázových úkolů, ale při vytváření, změně nebo odstranění více věcí, je procházení různých oken časově náročné. Výhodou příkazového řádku je, že v něm můžete rychle a efektivně vydávat příkazy nebo třeba používat skripty ke spuštění opakovaných úloh. V Azure máte dva různé nástroje příkazového řádku, se kterými můžete pracovat: Azure PowerShell a Azure CLI.

Oba tyto nástroje můžete použít k napsání skriptů, které kontrolují stav cloudových serverů, k nasazení nových konfigurací, otevření portů v bráně firewall nebo k připojení k virtuálnímu počítači kvůli změně nastavení. Správci Windows většinou dávají přednost Azure PowerShellu, zatímco vývojáři a správci Linuxu často používají Azure CLI.

V tomto modulu se zaměříme na používání Azure CLI k vytvoření a správě virtuálních počítačů hostovaných v Azure. Pokud si chcete udělat přehled o Azure CLI a chcete se dozvědět, jak tento nástroj nainstalovat a jak pracovat s předplatnými Azure, určitě se podívejte na školicí modul o **ovládání služeb Azure nástrojem CLI**.

## <a name="what-is-the-azure-cli"></a>Co je Azure CLI?

Azure CLI je nástroj příkazového řádku od Microsoftu pro správu prostředků Azure, který je určený pro různé platformy. Je k dispozici pro macOS, Linux a Windows a také v prohlížeči prostřednictvím [Azure Cloud Shellu](https://docs.microsoft.com/azure/cloud-shell/overview).

V Azure CLI můžete z příkazového řádku nebo pomocí skriptů spravovat prostředky Azure, jako jsou virtuální počítače a disky. Pojďme se rovnou podívat, co můžeme dělat s virtuálními počítači Azure.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Vytvořit virtuální počítač pomocí Azure CLI
- Měnit velikost virtuálních počítačů pomocí rozhraní příkazového řádku (CLI) Azure 
- Provádět základní úlohy správy pomocí rozhraní příkazového řádku (CLI) Azure 
- Připojit se k běžícímu virtuálnímu počítači pomocí SSH a Azure CLI

## <a name="prerequsites"></a>Předpoklady

- Základní znalost nástroje Azure CLI z modulu o **ovládání služeb Azure nástrojem CLI**.