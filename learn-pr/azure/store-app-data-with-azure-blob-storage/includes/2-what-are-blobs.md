Objekty blob jsou „soubory pro cloud“. Aplikace pracují s objekty blob podobným způsobem jako se soubory na disku, například při čtení a zápisu dat. Na rozdíl od místních souborů jsou ale objekty blob přístupné z libovolného místa s internetovým připojením.

Úložiště objektů blob v Azure je *nestrukturované*. To znamená, že neexistují žádná omezení typů dat, která se v něm dají ukládat. V objektu blob může být uložený například dokument PDF, obrázek JPG, soubor JSON, videoobsah atd. Objekty blob se neomezují jenom na běžné formáty souborů &mdash; mohou obsahovat gigabajty binárních dat z vědeckého přístroje, šifrovanou zprávu z jiné aplikace nebo třeba data ve vlastním formátu pro aplikaci, kterou vyvíjíte.

Objekty blob obvykle nejsou vhodné pro strukturovaná data, na která je potřeba se často dotazovat. Mají vyšší latenci než paměť a místní disk a nemají funkce indexování, díky kterým se v databázích efektivně zpracovávají dotazy. Objekty blob se ale často využívají v *kombinaci* s databázemi k ukládání dat bez možnosti dotazování. Například aplikace s databází uživatelských profilů může ukládat profilové obrázky v objektech blob. Každý uživatelský záznam v databázi by obsahoval název nebo adresu URL objektu blob, který obsahuje obrázek uživatele.

Objekty blob se používají pro různé způsoby ukládání dat napříč všemi typy aplikací a architektur:

- Aplikace, které potřebují přenášet velké objemy dat s využitím systémů zasílání zpráv, které podporují pouze malé zprávy. Tyto aplikace můžou ukládat data do objektů blob a ve zprávách odesílat jejich adresy URL.
- Úložiště objektů blob můžete použít jako systém souborů pro ukládání a sdílení dokumentů a dalších osobních údajů.
- Statické webové prostředky, jako jsou obrázky, se mohou ukládat v objektech blob a zpřístupnit k veřejnému stažení, jako kdyby to byly soubory na webovém serveru.
- Celá řada komponent Azure využívá objekty blob na pozadí. Například Azure Cloud Shell v objektech blob ukládá soubory a konfigurace a služba Azure Virtual Machines používá objekty blob pro úložiště na pevném disku.

Některé aplikace jako součást své práce neustále vytvářejí, aktualizují a odstraňují objekty blob. Jiné aplikace naproti tomu využívají jenom malou sadu objektů blob a mění je jenom zřídka.

## <a name="storage-accounts-containers-and-metadata"></a>Účty úložiště, kontejnery a metadata

V úložišti objektů blob je každý objekt blob umístěný v *kontejneru objektů blob*. Účet úložiště může obsahovat neomezený počet kontejnerů a v každém kontejneru může být neomezený počet objektů blob. Kontejnery jsou „ploché“ a mohou obsahovat jenom objekty blob, a ne další kontejnery.

Objekty blob a kontejnery podporují metadata ve formě řetězcových párů název-hodnota. Vaše aplikace mohou používat metadata pro cokoli: uživatelsky čitelný popis obsahu objektu blob, který má aplikace zobrazovat, řetězec, který vaše aplikace používá k určení způsobu zpracování dat objektu blob atd.

> [!TIP]
> Úložiště objektů blob neposkytuje žádný mechanismus pro vyhledávání objektů blob nebo jejich řazení podle metadat. V části Další materiály na konci tohoto modulu najdete informace o tom, jak k těmto účelům využít službu Azure Search.

## <a name="the-blob-storage-api-and-client-libraries"></a>Rozhraní API úložiště objektů blob a klientské knihovny

Rozhraní API úložiště objektů blob je založené na REST a podporují ho klientské knihovny v řadě oblíbených jazyků. Umožňuje psaní aplikací, které vytvářejí a odstraňují objekty blob a kontejnerů, nahrávají a stahují objekty blob a zobrazují seznam objektů blob v kontejneru.
