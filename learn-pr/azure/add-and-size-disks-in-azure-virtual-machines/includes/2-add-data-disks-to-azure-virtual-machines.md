Stejně jako každý jiný počítač i virtuální počítače v Azure používají pro uložení operačního systému, aplikací a dat disky. Tyto disky se nazývají virtuální pevné disky.

Předpokládejme, že jste v Azure vytvořili virtuální počítač, který bude hostitelem databáze s historií případů, na kterou vaše právnická firma spoléhá. Dobře navržená konfigurace disku je u SQL Serveru základ dobrého výkonu a odolnosti.

V této lekci se dozvíte, jak zvolit správné konfigurační hodnoty disků a jak tyto disky připojit k virtuálnímu počítači.

## <a name="how-disks-are-used-by-vms"></a>Použití virtuálních disků virtuálními počítači

Virtuální počítače používají disky ke třem různým účelům:

- **Uložiště operačního systému** – každý virtuální počítač obsahuje jeden disk s operačním systémem. Tento disk je zaregistrovaný jako jednotka SATA. Ve Windows je označený jako disk C: a v unixových operačních systémech je připojený v umístění „/“. Jeho maximální kapacita je 2048 GB (gigabajtů) a obsah se přebírá z image virtuálního počítače, kterou jste použili k jeho vytvoření.
- **Dočasné úložiště** – každý virtuální počítač obsahuje dočasný virtuální pevný disk, který se používá k uložení stránkovacích nebo odkládacích souborů. Po údržbě nebo opětovném nasazení může na tomto disku dojít ke ztrátě dat. Ve výchozím nastavení má tento disk na virtuálním počítači s Windows označení D:. Na tento disk neukládejte důležitá data, o která nechcete přijít.
- **Úložiště dat** – datový disk je každý další disk připojený k virtuálnímu počítači. Datové disky se používají k ukládání souborů, databází a dalších dat, které je potřeba mezi restarty zachovat. Některé image virtuálních počítačů obsahují datové disky ve výchozím nastavení. Můžete si také přidat další datové disky. Jejich maximální počet se odvíjí od velikosti virtuálního počítače. Každý datový disk je zaregistrovaný jako jednotka SCSI a jeho maximální kapacita je 4095 GB. U datových disků si můžete písmeno jednotky či přípojné body zvolit.

## <a name="storing-vhd-files"></a>Ukládání souborů virtuálního pevného disku

V Azure jsou virtuální pevné disky uložené v účtu Azure Storage jako **objekty blob stránky**.

Tato tabulka uvádí různé typy účtů úložiště a to, které objekty je pro ně možné využít.

|**Typ účtu úložiště**|**Standard pro obecné účely**|**Premium pro obecné účely**|**Blob Storage, horká a studená úroveň přístupu**|
|-----|-----|-----|-----|
|**Podporované služby**| Azure Blob Storage, Azure Files, Azure Queue Storage | Blob Storage | Blob Storage|
|**Typy podporovaných objektů blob**|Objekty blob bloku, objekty blob stránky a doplňovací objekty blob | Objekty blob stránky | Objekty blob bloku a doplňovací objekty blob|

Jak úložiště úrovně Standard pro obecné účely, tak i úložiště úrovně Premium podporují objekty blob stránky. Pokud je vaším hlavním hlediskem cena, zvolte účet úložiště úrovně Standard. Úložiště úrovně Premium bude stát více, ale bude také poskytovat vyšší rychlost vstupně-výstupních operací za sekundu (IOPS). Pokud je vaším požadavkem na virtuální počítač výkon dat, zvažte použití úložiště úrovně Premium.

## <a name="attach-data-disks-to-vms"></a>Připojení datových disků k virtuálním počítačům

Datové disky můžete k virtuálnímu počítači přidat kdykoli, stačí je připojit. Připojení disku přiřadí soubor virtuálního pevného disku k virtuálnímu počítači. 

Dokud je virtuální pevný disk připojený, nelze ho z účtu Storage odstranit.

### <a name="attach-an-existing-data-disk-to-an-azure-vm"></a>Připojení stávajícího datového disku k virtuálnímu počítači Azure

Možná nějaký virtuální pevný disk s uloženými daty, která chcete ve virtuálním počítači Azure použít, už máte. V našem scénáři právnické firmy jste už mohli například místně převést fyzické disky na virtuální pevné disky. V takovém případě můžete použít rutinu PowerShellu `Add-AzVhd` a nahrát je do účtu úložiště. Tato rutina je pro přenos souborů virtuálního pevného disku optimalizovaná a může nahrávání dokončit rychleji než jiné metody. Co nejlepší výkon přenosu zajišťuje několik vláken. Jakmile se virtuální pevný disk nahraje, připojte ho ke stávajícímu virtuálnímu počítači jako datový disk. Tento přístup je vynikajícím způsobem, jak do virtuálních počítačů Azure nasadit všechny typy dat. Tato data se ve virtuálním počítači objeví automaticky a na novém disku není potřeba vytvářet oddíly ani ho formátovat.

### <a name="attach-a-new-data-disk-to-an-azure-vm"></a>Připojení nového datového disku k virtuálnímu počítači Azure

Nový prázdný datový disk můžete k virtuálnímu počítači přidat pomocí webu Azure Portal. 

Tento proces vytvoří v požadovaném účtu úložiště soubor **VHD** jako objekt blob stránky a k virtuálnímu počítači ho připojí jako datový disk.

Než budete moct nový virtuální pevný disk použít k uložení dat, musíte ho inicializovat, vytvořit na něm oddíly a naformátovat ho. Tyto kroky si procvičíme v další lekci.

U fyzických místních serverů ukládáte data na fyzické pevné disky. U virtuálních počítačů Azure ukládáte data na virtuální pevné disky. Tyto virtuální pevné disky jsou uložené v účtech Azure Storage jako objekty blob stránky. Když například migrujete databázi historie případů své právnické firmy do Azure, musíte vytvořit virtuální pevné disky, do kterých se soubory databáze uloží.
