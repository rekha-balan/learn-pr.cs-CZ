Některé aplikace mají větší nároky na úložiště dat než jiné. Aplikace jako Dynamics CRM, Exchange Server, SAP Business Suite, SQL Server, Oracle a SharePoint vyžadují k optimálnímu běhu konstantně vysoký výkon a nízkou latenci.

Při vytváření virtuálních počítačů nebo přidávání nových disků máte několik možností, které budou mít výrazný dopad na výkon disku, počínaje _typem_ úložiště, který zvolíte.

## <a name="types-of-disks"></a>Typy disků 
Disky Azure jsou navržené pro 99,999% dostupnost. 

Při vytváření disků si můžete vybrat ze tří úrovní výkonu úložiště – disky SSD úrovně Premium, úložiště HDD úrovně Standard a SSD úrovně Standard. V závislosti na velikosti virtuálního počítače můžete kombinovat a párovat tyto typy disků nevidí.

### <a name="premium-ssd-disks"></a>Disky SSD úrovně Premium 

Disky SSD úrovně Premium jsou založené na jednotkách SSD (solid-state drive) a poskytují podporu vysoce výkonných disků s nízkou latencí pro virtuální počítače, na kterých se spouští náročné vstupně-výstupní úlohy. Tyto disky mají tendenci být spolehlivější, protože nemají žádné pohyblivé části. Aby se našla požadovaná data, nemusí se čtecí nebo zapisovací hlava přesouvat na správné místo na disku. 

Disky SSD úrovně Premium můžete používat s virtuálními počítači,jejichž velikost obsahuje v názvu řady „s“. Například existují řady **Dv3** a **Dsv3**, kdy **Dsv3** jde použít s disky SSD úrovně Premium.

### <a name="standard-hdd-storage"></a>Úložiště HDD úrovně Standard

Disky HDD úrovně Standard jsou zajišťované tradičními pevnými disky (HDD). Disky HDD úrovně Standard se účtují nižší sazbou než disky úrovně Premium. Disky HDD úrovně Standard je možné použít s jakoukoli velikostí virtuálního počítače.

### <a name="standard-ssd"></a>SSD úrovně Standard

Úložiště úrovně Premium je omezené na určité velikosti virtuálních počítačů, takže typ vytvořeného virtuálního počítače bude mít dopad na možnosti úložiště: velikost, maximální kapacitu a typ úložiště. Co když máte virtuální počítač nižší kategorie, ale potřebujete pro vstupně-výstupní výkon úložiště SSD? K tomu se právě používá SSD úrovně Standard. Disky SSD úrovně Standard se s ohledem na výkon a náklady nachází mezi standardními pevnými disky a prémiovými disky SSD.

Disky SSD úrovně Standard můžete použít s jakoukoli velikostí virtuálního počítače, včetně velikostí, které nepodporují úložiště úrovně Premium. Použití disků SSD úrovně Standard je jediným způsobem, jak na takových virtuálních počítačích využít disky SSD. Tento typ disku je k dispozici pouze v určitých oblastech a pouze se _spravovanými disky_.

### <a name="unmanaged-versus-managed-disks"></a>Porovnání nespravovaných a spravovaných disků

Když vytváříte virtuální počítače nebo virtuální pevné disky, máte možnost využívat **nespravované** nebo **spravované** disky.

S nespravovanými disky jste zodpovědní za účty úložiště, které se používají k uložení virtuálních pevných disků odpovídajících diskům virtuálních počítačů. Poplatky za účet úložiště se platí podle používaného místa. Jeden účet úložiště má pevný rychlostní limit 20 000 vstupně-výstupních operací za sekundu. To znamená, že při plném výkonu dokáže podporovat 40 standardních virtuálních pevných disků. Pokud potřebujete horizontálně navýšit kapacitu, potřebujete více účtů úložiště, což může být složité.

Spravované disky představují novější, **doporučovaný model diskového úložiště**. Elegantně řeší tyto komplikace tím, že problémy se správou účtů úložiště nechají na Azure. Vy určíte typ disku a jeho velikost a Azure se postará o vytvoření a správu disku _i_ úložiště, které používá. Nemusíte se starat o omezení účtu úložiště, což usnadňuje horizontální navýšení kapacity. Tady jsou některé z výhod, které získáte oproti starším nespravovaným diskům:

- **Větší spolehlivost**: Azure zajistí, aby virtuální pevné disky používané pro vysoce spolehlivé virtuální počítače byly umístěny v různých částech Azure Storage kvůli zajištění podobné míry odolnosti.
- **Lepší zabezpečení**: Spravované disky jsou spravovanými prostředky ve skupině prostředků. To znamená, že je možné použít řízení přístupu na základě rolí a omezit tak, kdo může pracovat s daty na virtuálním pevném disku.
- **Podpora snímků**: K vytvoření kopie virtuálního pevného disku, která je jen pro čtení, můžete použít snímky. Je při tom potřeba vypnout nadřízený virtuální počítač, ale vytvoření snímku trvá jen pár sekund. Jakmile je snímek vytvořený, můžete virtuální počítač zase zapnout. Snímek můžete použít k vytvoření duplicitního virtuálního počítače kvůli řešení potíží s produkčním systémem nebo můžete vrátit virtuální počítač do okamžiku pořízení snímku.
- **Podpora zálohování**: Spravované disky můžete službou Azure Backup automaticky zálohovat do různých oblastí, abyste je v případě havárie mohli obnovit. Zálohování nemá vliv na službu virtuálního počítače.

Kvůli všem dalším výhodám, včetně zaručené výkonové charakteristiky, byste měli pro nové virtuální počítače vždy vybrat spravované disky.

### <a name="disk-comparison"></a>Porovnání disků
Následující tabulka obsahuje porovnání HDD úrovně Standard, SSD úrovně Standard a SSD úrovně Premium, aby vám pomohla při rozhodování, co použít.

|    | Disk Azure Premium |Disk SSD úrovně Azure Standard (Preview)| Disk HDD úrovně Azure Standard 
|--- | ------------------ | ------------------------------- | ----------------------- 
| **Typ disku** | Jednotky SSD (solid-state drive) | Jednotky SSD (solid-state drive) | Jednotky pevných disků (HDD)  
| **Přehled**  | Podpora disků založených na jednotkách SSD s vysokým výkonem a nízkou latencí pro virtuální počítače, na kterých se spouští náročné vstupně-výstupní úlohy nebo které hostují kriticky důležité produkční prostředí |Konzistentnější výkon a spolehlivost než pevné disky. Optimalizované pro úlohy s nízkým počtem IOPS| Nákladově efektivní disk založený na pevném disku pro úlohy s řídkým přístupem
| **Scénář**  | Úlohy v produkčním prostředí a úlohy, u kterých záleží na výkonu |Webové servery, málo používané podnikové aplikace a vývoj/testování| Zálohování, nekritické, řídký přístup 
| **Velikost disku** | 32-4095 GiB | 128-4095 GiB | 32-4095 GiB 
| **Maximální propustnost na disk** | 250 MiB/s | Až 60 MiB/s | Až 60 MiB/s 
| **Maximum vstupně-výstupních operací za sekundu (IOPS) na disk** | 7500 IOPS | Až 500 IOPS | Až 500 IOPS 

Podrobnosti o výkonu disků jsou uvedené níže.

## <a name="data-replication"></a>Replikace dat

Data na vašem účtu Microsoft Azure Storage se vždy replikují, aby se zajistila jejich stálost a vysoká dostupnost. Replikace Azure Storage vaše data zkopíruje, aby byla chráněná před plánovanými i neplánovanými událostmi, jako jsou krátkodobá selhání hardwaru, výpadky sítě či napájení, přírodní katastrofy atd. Data můžete replikovat v rámci stejného datacentra, napříč zónovými datacentry ve stejné oblasti a dokonce napříč oblastmi. Existují čtyři typy replikace:

- **Místně redundantní úložiště (LRS)** – Azure replikuje data v rámci stejného datacentra Azure. Když uzel selže, zůstávají data dostupná. Pokud však selže celé datacentrum, nemusí být data k dispozici.
- **Geograficky redundantní úložiště (GRS)** – Azure replikuje data do druhé oblasti, která je od primární oblasti vzdálená stovky kilometrů. Pokud váš účet úložiště používá GRS, budou vaše data stálá i v případě, že dojde k výpadku nebo katastrofě v celé oblasti a primární oblast se nepodaří obnovit.
- **Geograficky redundantní úložiště jen pro čtení (RA-GRS)** – Azure poskytuje k datům v sekundárním umístění přístup jen pro čtení a georeplikaci napříč dvěma oblastmi. Pokud datacentrum selže, budete moct data číst, ale nebudete je moct upravovat.
- **Zónově redundantní úložiště (ZRS)** – Azure replikuje data synchronně mezi třemi clustery úložiště v jedné oblasti. Každý cluster úložiště je fyzicky oddělený od ostatních a je umístěný ve své vlastní zóně dostupnosti. U tohoto typu replikace můžete v případě nedostupné zóny stále k datům přistupovat a spravovat je.

Účty úložiště úrovně Standard podporují všechny druhy replikace, ale účty úložiště úrovně Premium podporují pouze místně redundantní úložiště (LRS). Vzhledem k tomu, že samotné virtuální počítače běží v jedné oblasti, nebývá toto omezení většinou problém.

## <a name="disk-performance"></a>Výkon disků

Výkon disků závisí na typu disku, který jste zvolili. Každý disk je hodnocen podle konkrétního počtu vstupně-výstupních operací za sekundu (IOPS). Kromě toho má každá jednotka hodnocení propustnosti, které určuje, kolik dat můžete číst nebo zapisovat během sekundy. Kombinace těchto dvou faktorů určuje, jak je disk rychlý.

S úložištěm úrovně Standard získáte maximální propustnost **500 IOPS a 60 MB za sekundu** na disk (dokonce i na jednotkách SSD). S úložištěm úrovně Premium závisí počet IOPS na discích úrovně Premium, které zvolíte, a velikosti virtuálního počítače.

|  | P4 | P6 | P10 | P20 | P30 | P40 | P50 |
|--|----|----|-----|-----|-----|-----|-----|
| **Velikost disku** | 32 GiB | 64 GiB | 128 GiB | 512 GiB | 1 TiB | 2 TiB | 4 TiB |
| **Maximum vstupně-výstupních operací za sekundu (IOPS) na disk** | 120 | 240 | 500 | 2300 | 5000 | 7500 | 7500 |
| **Maximální propustnost na disk** | 25 MB/s | 50 MB/s | 100 MB/s | 200 MB/s | 250 MB/s | 250 MB/s |

Jak sami vidíte, máte rozpětí od **25 MB/s** a **120 IOPS** do **250 MB/s** a **7500 IOPS**.