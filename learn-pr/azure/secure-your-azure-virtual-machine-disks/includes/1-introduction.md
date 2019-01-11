Předpokládejme, že pracujete pro společnost skladu, která přechází na cloud. V současné době používáte hybridní prostředí, které se skládá z místních serverů s Windows, virtuálních počítačů Azure a služby Azure Active Directory. Vaše společnost vyvinula vlastní interní infrastrukturu B2B (business-to-business), která podporuje zabezpečenou správu objednávek ve spolupráci s dodavateli. Někteří z vašich dodavatelů používají servery s Linuxem a vy pro podporu těchto dodavatelů v Azure provozujete několik serverů s Linuxem.

Vaše zásady zabezpečení vyžadují, aby byla data šifrována pomocí vašich vlastních šifrovacích klíčů a aby za správu těchto klíčů zodpovídala vaše společnost.

Váš tým správců už používá PowerShell pro správu místního serveru. Nasadíte a otestujete mnoho virtuálních počítačů Azure a vaším záměrem je automatizovat tento proces pomocí šablon Azure Resource Manageru.

Tady se podíváme na typy ochrany, které jsou k dispozici pro disky virtuálních počítačů, abyste se mohli rozhodnout, jestli je Azure Disk Encryption (ADE) tou nejlepší volbou pro daný scénář. Pak povolíme službu ADE na existujících discích virtuálních počítačů a pomocí šablon povolíme službu ADE pro nová nasazení virtuálních počítačů.


## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Určit, jaká metoda šifrování je nejvhodnější pro váš virtuální počítač
- Šifrování existující disky virtuálních počítačů pomocí portálu Azure Portal
- Šifrovat existující disky virtuálních počítačů pomocí PowerShellu
- Upravit šablony Azure Resource Manageru pro automatizaci šifrování disků u nových virtuálních počítačů
