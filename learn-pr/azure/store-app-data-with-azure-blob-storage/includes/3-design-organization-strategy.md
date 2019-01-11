Při návrhu aplikace, která potřebuje ukládat data, je důležité zvážit, jak bude aplikace uspořádávat data do různých účtů úložiště, kontejnerů a objektů blob.

## <a name="storage-accounts"></a>Účty úložiště

Jediný účet úložiště je dostatečně flexibilní, abyste si v něm mohli uspořádat objekty blob podle svých požadavků, ale podle potřeby se doporučuje používat i další účty úložiště za účelem logického oddělení nákladů a řízení přístupu k datům.

## <a name="containers-and-blobs"></a>Kontejnery a objekty blob

Vaše strategie pojmenování a uspořádání kontejnerů a objektů blob by měla vycházet z povahy vaší aplikace a dat, která ukládá.

Aplikace využívající objekty blob v rámci schématu úložiště, které obsahuje databázi, často příliš nepotřebují čerpat informace o datech z jejich uspořádání, názvů nebo metadat. V takových aplikacích se jako názvy objektů blob běžně používají identifikátory GUID a na tyto identifikátory odkazují záznamy v databázi. Aplikace potom pomocí databáze určuje, kde jsou objekty blob uložené a jaký druh dat obsahují.

Jiné aplikace zase můžou používat službu Azure Blob Storage spíš jako osobní systém souborů, ve kterém názvy kontejnerů a objektů blob nesou význam a vypovídají o struktuře. Názvy objektů blob v těchto typech aplikací často vypadají jako klasické názvy souborů a obsahují přípony názvů souborů jako `.jpg`, které udávají, jaký druh dat obsahují. K uspořádání objektů blob používají virtuální adresáře (viz níže) a často ukládají informace o objektech blob a kontejnerech do značek metadat.

Při rozhodování o tom, jak uspořádat a ukládat objekty blob a kontejnery, je potřeba zvážit několik základních bodů.

### <a name="naming-limitations"></a>Omezení pojmenování

Názvy kontejnerů a objektů blob se musí řídit sadou pravidel, včetně omezení délky a povolených znaků. V části Další materiály na konci tohoto modulu najdete konkrétnější informace o pravidlech pojmenování.

### <a name="public-access-and-containers-as-security-boundaries"></a>Veřejný přístup a kontejnery jako hranice zabezpečení

Ve výchozím nastavení je potřeba k přístupu ke všem objektům blob provést ověření. Jednotlivé kontejnery je ale možné nakonfigurovat tak, aby umožňovaly veřejné stahování obsažených objektů blob bez ověření. Tato funkce je využitelná v mnoha případech, třeba při hostování prostředků statického webu a sdílení souborů. Důvodem je to, že stahování obsahu objektů blob funguje stejně jako čtení jakéhokoli jiného druhu dat přes web: stačí nasměrovat na adresu URL objektu blob prohlížeč nebo jiný nástroj, který dokáže zadat požadavek GET.

Povolení veřejné adresy je důležité z hlediska škálovatelnosti, protože data stahovaná přímo z úložiště objektů blob negenerují žádné přenosy dat v aplikaci na straně serveru. I když veřejný přístup hned nevyužijete nebo budete řídit přístup k datům ve vaší aplikaci prostřednictvím databáze, naplánujte použití oddělených kontejnerů pro data, která budete chtít mít veřejně dostupná.

> [!CAUTION]
> Objekty blob v kontejneru nakonfigurovaném pro veřejný přístup může bez jakéhokoli ověření nebo auditu stáhnout kdokoli, kdo zná jejich adresy URL v úložišti. Do veřejného kontejneru nikdy neukládejte data objektů blob, která nechcete veřejně sdílet.

Kromě veřejného přístupu nabízí Azure funkci sdíleného přístupového podpisu, která umožňuje podrobnou správu oprávnění v kontejnerech. Přesné řízení přístupu umožňuje pracovat se scénáři, které ještě zlepšují škálovatelnost, takže je dobré přemýšlet o kontejnerech obecně jako o hranicích zabezpečení.

### <a name="blob-name-prefixes-virtual-directories"></a>Předpony názvů objektů blob (virtuální adresáře)

Technicky vzato jsou kontejnery „ploché“ a nepodporují žádný druh vnoření ani hierarchie. Pokud ale dáte objektům blob hierarchické názvy, které připomínají cesty k souboru (třeba `finance/budgets/2017/q1.xls`), dokáže operace výpisu v rozhraní API filtrovat výsledky podle konkrétních předpon. To vám umožní orientovat se ve výpisu jako v hierarchickém systému souborů a složek.

Tato funkce se často označuje jako *virtuální adresáře*, protože ji některé nástroje a klientské knihovny používají k vizualizaci úložiště objektů blob a k pohybu v něm, jako by se jednalo o systém souborů. Každá navigace ve složce aktivuje samostatné volání výpisu objektů blob v dané složce.

Používání názvů objektů blob, které připomínají názvy souborů, je běžný postup sloužící k uspořádání složitých dat objektů blob a k orientaci v nich.

### <a name="blob-types"></a>Typy objektů blob

Existují tři různé typy objektů blob, do kterých můžete ukládat data:

- **Objekty blob bloku** se skládají z bloků různých velikostí, které se dají nezávisle na sobě nahrávat, a to i souběžně. Při zápisu do objektu blob bloku dochází k nahrávání dat do bloků a potvrzení jejich předání do objektu blob.
- **Doplňovací objekty blob** jsou specializované objekty blob bloku, které podporují jenom doplňování nových dat (ne aktualizaci ani odstraňování stávajících dat), ale zato to dělají velmi efektivně. Doplňovací objekty blob jsou velmi vhodné pro scénáře, jako je ukládání protokolů nebo zapisování streamovaných dat.
- **Objekty blob stránky** jsou určené pro scénáře, které zahrnují čtení a zápis s náhodným přístupem. Objekty blob stránky se používají k ukládání souborů virtuálního pevného disku (VHD) používaného službou Azure Virtual Machines, ale hodí se pro každý scénář, který zahrnuje náhodný přístup.

Objekty blob bloku jsou ideální pro většinu scénářů, které výslovně nevyžadují doplňovací objekty blob nebo objekty blob stránky. Jejich struktura založená na blocích podporuje velmi rychlé nahrávání i stahování a efektivní přístup k jednotlivým částem objektu blob. Správa a potvrzování bloků probíhá ve většině klientských knihoven automaticky a některé knihovny také maximalizují výkon paralelním nahráváním a stahováním.