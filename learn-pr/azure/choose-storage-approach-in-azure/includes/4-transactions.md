Aplikace můžou potřebovat seskupit řadu aktualizací dat dohromady, protože změna v jedné části dat musí vést ke změně v jiné části. Transakce vám umožní tyto aktualizace seskupit, což znamená, že pokud selže jedna událost v řadě aktualizací, je možné vrátit zpět celou řadu. 

Jako online obchodník byste například mohli transakci použít pro seskupení odeslání objednávky a ověření platby. Seskupením souvisejících událostí dosáhnete toho, že se nezmění informace o stavu skladových zásob, dokud systém neobdrží schválený způsob platby.

Tady se seznámíte s transakcemi a zjistíte, jestli je pro svá data potřebujete.

## <a name="what-is-a-transaction"></a>Co je transakce?

Transakce je logická skupina databázových operací, které se provádějí společně.

Co se týče rozhodování o tom, jestli ve své aplikaci potřebujete použít transakce, musíte si položit následující otázku: Bude mít změna v jedné části dat v sadě vliv na jinou část dat? Pokud je odpověď kladná, pak potřebujete, aby vaše databázová služba podporovala transakce.

Transakce se často definují sadou čtyř požadavků, kterým se také říká záruky ACID. ACID (zkratka z anglického Atomicity, Consistency, Isolation, Durability) značí nedělitelnost, konzistenci, izolaci a stálost:

- Atomicita znamená, že buď se provedou všechny operace v transakci, nebo žádná z nich.
- Konzistence zaručuje, že pokud se v průběhu transakce něco stane, nedojde k tomu, že by se část dat neaktualizovala. Obecně platí, že se transakce použije konzistentně, nebo vůbec.
- Izolace zajišťuje, že jedna transakce nemá vliv na žádnou jinou transakci.
- Stálost znamená, že změny provedené v důsledku transakce se trvale uloží v systému. Systém uloží zapsaná data, takže i kdyby došlo k selhání a restartování systému, budou data k dispozici ve správném stavu.

Pokud databáze nabízí záruky ACID, uplatňují se tyto principy konzistentně u všech transakcí.

## <a name="oltp-vs-olap"></a>OLTP vs. OLAP

Transakčním databázím se často říká systémy online zpracování transakcí neboli OLTP. Systémy OLTP běžně podporují mnoho uživatelů, rychle reagují a zpracovávají obrovské objemy dat. Jsou také vysoce dostupné (to znamená, že mají minimální výpadky) a obvykle zpracovávají malé nebo relativně jednoduché transakce.

Naproti tomu systémy OLAP (Online Analytical Processing) většinou podporují menší množství uživatelů, mají delší dobu odezvy, můžou být méně dostupné a obvykle zpracovávají velké a složité transakce.

Termíny OLTP a OLAP se už nepoužívají tak často jako dřív, ale jejich porozumění vám usnadní kategorizaci požadavků pro vaši aplikaci. 

Když jste se teď seznámili s transakcemi a s technologiemi OLTP a OLAP, pojďme si projít každou sadu dat ve scénáři online obchodu a rozhodnout se, jestli jsou u ní transakce zapotřebí.

## <a name="product-catalog-data"></a>Data v katalogu produktů

Data v katalogu produktů by měla být uložena v transakční databázi. Jakmile uživatel odešle objednávku a ověří se platba, měl by se aktualizovat stav skladových zásob pro danou položku. Stejně tak platí, že pokud dojde k odmítnutí zákazníkovy platební karty, měla by být objednávka vrácena zpět a stav skladových zásob by měl zůstat nezměněný. Všechny tyto relace vyžadují transakce.

## <a name="photos-and-videos"></a>Fotky a videa

Fotky a videa v katalogu produktů nevyžadují podporu transakcí. Tyto soubory se změní pouze při aktualizaci nebo přidání nových souborů. Přestože mezi obrázky a samotnými daty o produktu existuje relace, není ze své podstaty transakční.

## <a name="business-data"></a>Firemní data

Firemní data jsou historická a neměnná a nevyžadují tedy podporu transakcí. Firemní analytici, kteří s daty pracují, mají navíc specifické požadavky v tom smyslu, že ve svých dotazech často potřebují používat agregace, aby mohli pracovat s celkovými hodnotami menších datových bodů.

## <a name="summary"></a>Shrnutí

Není vždy jednoduché zajistit, aby vaše data byla ve správném stavu. Transakce vám v tom můžou pomoct tím, že pro vaše data vynucují požadavky na integritu dat. Pokud jsou pro vaše data přínosem principy ACID, zvolte řešení úložiště, které podporuje transakce.