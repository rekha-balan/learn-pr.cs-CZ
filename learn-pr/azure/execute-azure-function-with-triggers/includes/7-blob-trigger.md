Představte si, že jste fotograf a máte web, kde zobrazujete vaše každodenní obrázky. Protože máte spoustu práce, nemáte konzistentní plán nahrávání, ale chcete mít možnost upozornit vaše fanoušky, když nahrajete nový obrázek. Rozhodnete se vytvořit funkci Azure, která při každém nahrání obrázku do kontejneru objektů blob služby Azure Storage automaticky odešle tweet.

Tady se naučíte, jak vytvořit trigger objektu blob a instruovat ho, aby monitoroval konkrétní umístění v kontejneru objektů blob služby Azure Storage.

## <a name="what-is-azure-storage"></a>Co je Azure Storage?

Azure Storage je řešení cloudového úložiště od Microsoftu, které podporuje všechny typy dat, včetně objektů blob, front a NoSQL. Cílem služby Azure Storage je poskytovat úložiště dat, které je:

- Vysoce dostupné
- Zabezpečené
- Škálovatelné
- Spravované

Službou Azure Storage se ale nebudeme moc zabývat. Použijeme ji jenom k vytvoření objektů blob, které budou aktivovat spuštění naší funkce.

## <a name="what-is-azure-blob-storage"></a>Co je úložiště objektů blob v Azure?

Úložiště objektů blob v Azure je řešení úložiště objektů, které je navržené pro ukládání velkých objemů nestrukturovaných dat. 

Úložiště objektů blob v Azure je například skvělé pro následující úlohy:

- Ukládání souborů
- Poskytování souborů
- Streamování videa a zvuku
- Protokolování dat

Existují tři typy objektů blob: **objekty blob bloku**, **doplňovací objekty blob** a **objekty blob stránky**. Nejběžnějším typem jsou objekty blob bloku. Umožňují efektivní ukládání textu a binárních dat. Doplňovací objekty blob jsou podobné jako objekty blob bloku, ale jsou navržené spíš pro operace přidávání, jako je vytvoření souboru protokolu, který se neustále aktualizuje. Konečně objekty blob stránky jsou tvořené stránkami a jsou navržené pro časté operace náhodného čtení nebo zápisu.

## <a name="what-is-a-blob-trigger"></a>Co je trigger objektu blob?

Trigger objektu blob je aktivační událost, která spustí funkci, když se v úložišti objektů blob v Azure načte nebo aktualizuje soubor. Pokud chcete vytvořit trigger objektu blob, vytvoříte účet služby Azure Storage a zadáte umístění, které tento trigger monitoruje.

## <a name="how-to-create-a-blob-trigger"></a>Jak vytvořit trigger objektu blob

Stejně jako ostatní triggery, se kterými jsme se dosud setkali, vytvoříme i trigger objektu blob na webu Azure Portal. Uvnitř vaší funkce Azure v seznamu předdefinovaných typů triggerů vyberte **Trigger objektu blob**. Potom zadejte logiku, která se má spustit, když se vytvoří nebo aktualizuje objekt blob.

Jedním z nastavení, kterým musíte věnovat pozornost, je nastavení **Cesta**. **Cesta** říká triggeru objektu blob, kde má monitorovat, aby zjistil, jestli se načetl nebo aktualizoval objekt blob. Ve výchozím nastavení má **Cesta** tuto hodnotu: 

> samples-workitems/{name}

Rozdělme si teď tuto hodnotu na dvě části: *samples-workitems* a *{name}*. První část, *samples-workitems*, představuje kontejner objektů blob, který tento trigger monitoruje. Druhá část, *{name}* , určuje, že vyvolání příslušné funkce aktivační událostí způsobí libovolný typ souboru. Tato funkce se vyvolá, protože neexistuje žádný filtr. Tento trigger se dá například nastavit tak, aby vyvolal příslušnou funkci jenom při přidání souboru PNG, a to pomocí syntaxe:

> samples-workitems/{name}.png

Poslední důležitou informací je text *name*. Text *name* reprezentuje parametr ve vaší funkci Azure, který přijímá název přidaného souboru. Pokud například nahrajete soubor s názvem *resume.txt*, funkce Azure přijme tuto hodnotu jako řetězec, a to prostřednictvím parametru nazvaného *name*.

Trigger objektu blob vyvolá funkci Azure, jakmile zjistí aktivitu v konkrétním umístění vašeho účtu služby Azure Storage. Umístění, které se má monitorovat, zadáte úpravou hodnoty **Cesta** na webu Azure Portal.