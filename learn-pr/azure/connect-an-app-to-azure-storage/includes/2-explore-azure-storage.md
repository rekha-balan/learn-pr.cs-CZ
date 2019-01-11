Pojďme se podívat na služby Azure Storage Services, styly dat a účty. 

Microsoft Azure Storage je spravovaná služba, která poskytuje odolné, zabezpečené a škálovatelné úložiště v cloudu. Pojďme se na tyto termíny podívat podrobněji.

| | |
|-|-|
| **Odolné** | Redundance zajišťuje, že vaše data budou v případě krátkodobého selhání hardwaru v bezpečí. Můžete také replikovat data napříč několika datacentry nebo geografickými oblastmi, abyste se ochránili i proti místní pohromě nebo přírodní katastrofě. Data replikovaná tímto způsobem zůstávají v případě nečekaného výpadku vysoce dostupná. |
| **Zabezpečené** | Všechna data zapsaná do služby Azure Storage jsou touto službou šifrovaná. Azure Storage poskytuje jemně odstupňované řízení přístupu k datům. |
| **Škálovatelné** | Služba Azure Storage je navržená pro širokou škálovatelnost, aby splňovala požadavky dnešních aplikací na datové úložiště a výkon. |
| **Spravované** | Microsoft Azure se stará o údržbu a řeší za vás veškeré kritické problémy. |

Jedno předplatné Azure může hostovat až 200 účtů úložiště, z nichž každý může obsahovat až 500 TB dat. Pokud máte obchodní případ, můžete kontaktovat tým Azure Storage a získat schválení až pro 250 účtů úložiště v předplatném, čímž dosáhnete maximálního úložiště až 125 petabajtů.

## <a name="azure-data-services"></a>Datové služby Azure

Azure Storage zahrnuje čtyři typy dat:

- **Objekty blob**: široce škálovatelné úložiště objektů pro textová a binární data
- **Soubory**: spravované sdílené složky pro nasazení v cloudu nebo místní nasazení
- **Fronty**: spolehlivé úložiště pro přenos zpráv mezi součástmi aplikace
- **Tabulky**: úložiště NoSQL pro úložiště strukturovaných dat bez schématu Tuto službu nahradila služba Cosmos DB a nebude zde popsána.

Všechny tyto datové typy ve službě Azure Storage jsou přístupné prostřednictvím protokolu HTTP nebo HTTPS odkudkoli na světě. Společnost Microsoft poskytuje sady SDK pro Azure Storage v nejrůznějších jazycích, stejně jako rozhraní REST API. Data můžete také vizuálně prozkoumat přímo na webu Azure Portal.

### <a name="blob-storage"></a>Blob Storage

Azure Blob Storage je řešení úložiště objektů optimalizované pro ukládání velkých objemů nestrukturovaných dat, jako jsou textová nebo binární data. Blob Storage je ideální pro:

- Poskytování obrázků nebo dokumentů přímo do prohlížeče, včetně plně statických webů
- Ukládání souborů pro distribuovaný přístup
- Streamování videa a zvuku
- Ukládání dat pro zálohování a obnovování, zotavení po havárii a pro archivaci
- Ukládání dat, která bude analyzovat místní nebo v Azure hostovaná služba

Azure Storage podporuje tři druhy objektů blob:

| Typ objektu blob | Popis |
|-----------|-------------|
| **Objekty blob bloku** | Objekty blob bloku se používají k uložení textových nebo binárních souborů až do velikosti 5 TB (50 000 bloků po 100 MB). Objekty blob bloku se primárně používají pro úložiště souborů, které se čtou od začátku do konce, jako jsou například mediální soubory nebo soubory obrázků pro weby. Označují se jako objekty blob bloku, protože soubory větší než 100 MB se musí nahrávat jako malé bloky, které se potom konsolidují (nebo potvrzují) do koncového objektu blob. |
| **Objekty blob stránky** | Objekty blob stránky se používají k uložení souborů s náhodným přístupem až do velikosti 8 TB. Objekty blob stránky se primárně používají jako záložní úložiště pro virtuální pevné disky používané k poskytování odolných disků pro Azure Virtual Machines (virtuální počítače Azure). Označují se jako objekty blob stránky, protože poskytují náhodný přístup pro čtení/zápis k 512bajtovým stránkám. |
| **Doplňovací objekty blob** | Doplňovací objekty blob jsou složené z bloků podobně jako objekty blob bloku, ale jsou optimalizované pro připojovací operace. Často se používají pro protokolování informací z jednoho nebo více zdrojů do stejného objektu blob. Můžete například zapsat veškeré protokolování trasování do stejného doplňovacího objektu blob pro aplikaci spuštěnou na více virtuálních počítačích. Jeden doplňovací objekt blob může mít až 195 GB. |

### <a name="files"></a>Soubory

Služba Soubory Azure umožňuje nastavit vysoce dostupné sdílené složky souborů sítě, ke kterým je možný přístup pomocí standardního protokolu SMB (Server Message Block). To znamená, že několik virtuálních počítačů může sdílet stejné soubory s oprávněním ke čtení i zápisu. Soubory můžete číst také pomocí rozhraní REST nebo klientských knihoven pro úložiště. Jakémukoli souboru také můžete přiřadit jedinečnou adresu a po nastavené časové období umožnit jemně odstupňovaný přístup k privátnímu souboru. Sdílené složky můžete použít pro řadu běžných scénářů:

- Ukládání sdílených konfiguračních souborů pro virtuální počítače nebo nástroje, aby všichni používali stejnou verzi.
- Soubory protokolů, například diagnostiky, metriky a výpisy stavu systému.
- Data sdílená mezi místními aplikacemi a virtuálními počítači Azure za účelem umožnění migrace aplikací do cloudu během časového období.

### <a name="queues"></a>Fronty

Služba front Azure se využívá k ukládání a načítání zpráv. Fronty zprávy mohou mít velikost až 64 kB a jedna fronta může obsahovat miliony zpráv. Fronty se obecně používají k ukládání seznamů zpráv, které mají být zpracovány asynchronně.

Fronty můžete použít k volnému propojení různých částí vaší aplikace. Mohli bychom například provést zpracování obrázků nahraných našimi uživateli. Možná bychom chtěli poskytnout nějakou formu detekce obličeje nebo možnost označení, aby lidé mohli prohledávat veškeré snímky, které v naší službě uložili. Fronty bychom mohli použít k předávání zpráv naší službě pro zpracování obrázků, abychom jí oznámili, že byly nahrány nové obrázky a že jsou připravené ke zpracování. Tento druh architektury by vám umožnil nezávisle vyvíjet a aktualizovat jednotlivé části služby.

## <a name="azure-storage-accounts"></a>Účty služby Azure Storage

Abyste mohli k jakékoli z těchto služeb přistupovat z aplikace, musíte si vytvořit _účet úložiště_. Účet úložiště poskytuje jedinečný obor názvů v Azure pro ukládání datových objektů a přístup k nim. Účet úložiště obsahuje všechny objekty blob, soubory, fronty, tabulky a disky virtuálních počítačů, které pod tímto účtem vytvoříte.

### <a name="creating-a-storage-account"></a>Vytvoření účtu úložiště

Účet Azure Storage si můžete vytvořit pomocí webu Azure Portal, Azure PowerShellu nebo Azure CLI. Azure Storage poskytuje tři různé možnosti účtu s různými cenami a podporovanými funkcemi.

> [!div class="mx-tableFixed"]
> | Typ účtu | Popis |
> |--------------|-------------|
> | **Pro obecné účely verze 2 (GPv2)** | Účty pro obecné účely verze 2 (GPv2) jsou účty úložiště, které podporují všechny nejnovější funkce pro objekty blob, soubory, fronty a tabulky. Ceny za účty GPv2 byly navržené pro zajištění nejnižších cen za gigabajt. |
> | **Pro obecné účely verze 1 (GPv1)** | Účty pro obecné účely verze 1 (GPv1) poskytují přístup ke všem službám Azure Storage, ale nemusí zahrnovat nejnovější funkce nebo nejnižší ceny za gigabajt. Účty GPv1 například nepodporují studené úložiště a úložiště archivu. Ceny za transakce jsou u účtů GPv1 nižší, takže z tohoto typu účtu můžou mít užitek úlohy s vysokou četností změn dat nebo vysokou frekvencí čtení. |
> | **Účty úložiště Blob Storage** | Účty úložiště Blob Storage jsou starší typ účtů a podporují stejné funkce objektů blob bloku jako účty GPv2, ale jsou omezené pouze na objekty blob bloku a doplňovací objekty blob. Ceny jsou velmi podobné cenám za účty pro obecné účely verze 2. |

Pokud se chcete dozvědět více informací o vytváření účtů úložiště, nezapomeňte se podívat na modul **Vytvoření účtu služby Azure Storage** na výukovém portálu.