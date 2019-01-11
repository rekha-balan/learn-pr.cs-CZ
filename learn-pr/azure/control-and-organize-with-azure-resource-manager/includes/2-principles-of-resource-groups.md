Ve svém prvním týdnu v nové práci jste si prošli prostředky, které jsou aktuálně vytvořené ve vašem firemním předplatném Azure. Zjistili jste, že se v něm nachází mnoho skupin prostředků se spoustou různých prostředků, nejsou ale uspořádané do žádné koherentní struktury. Azure už trochu znáte, ale přesně nevíte, jak skupiny prostředků fungují a jaká je jejich úloha. Odhadli jste ale (správně), že můžou být užitečné při uspořádání prostředků. Pojďme se tedy se skupinami prostředků a s jejich využitím seznámit podrobněji.

Pokud si budete chtít postupy vyzkoušet s námi, můžete v tomto modulu použít svoje vlastní předplatné. Budeme pracovat s prostředky, s kterými se nepojí žádné náklady, takže vám ke cvičením postačí zkušební verze předplatného, případně předplatné, které už máte.

[!INCLUDE [azure-free-trial-note](../../../includes/azure-free-trial-note.md)]

## <a name="what-are-resource-groups"></a>Co jsou skupiny prostředků?

Skupiny prostředků jsou jedním ze základních prvků platformy Azure. Skupina prostředků je logický kontejner pro prostředky nasazené v Azure. Prostředek je cokoli, co v předplatném Azure vytvoříte: virtuální počítače, brány Application Gateway, instance služby Cosmos DB a podobně. Všechny prostředky musí být umístěny ve skupině prostředků. Každý přitom může být členem jen jedné skupiny prostředků. Prostředky lze mezi skupinami prostředků kdykoli přesunout. Skupiny prostředků se nedají vnořit do sebe. Než prostředek zřídíte, potřebujete skupinu prostředků, kam ho umístíte.

### <a name="life-cycle"></a>Životní cyklus

Odstraněním skupiny prostředků se odstraní také všechny prostředky v ní obsažené. Uspořádat prostředky podle životního cyklu se může hodit v neprodukčních prostředích, kde zkoušíte nějaký experiment a jakmile jste s ním hotoví, zbavíte se ho. Skupina prostředků umožňuje snadno odebrat sadu prostředků najednou.

### <a name="authorization"></a>Autorizace 

Skupiny prostředků zároveň představují obor pro přiřazení oprávnění pro řízení přístupu na základě rolí. Uplatněním těchto oprávnění na skupinu prostředků si zjednodušíte správu a omezíte přístup tak, abyste uživatelům povolili jen to, co potřebují.

### <a name="logical-grouping"></a>Logické seskupení

Smyslem skupin prostředků je pomoct vám se správou a uspořádáním prostředků Azure. Když dáte dohromady prostředky s podobným využitím, podobného typu nebo v podobném umístění, vnesete do prostředků, které v Azure vytváříte, určitý řád. Logické seskupení je aspekt, který nás v tomto příkladu bude zajímat nejvíce, protože naše prostředky jsou značně neuspořádané.

![Alternativní text](../media/2-rg.PNG)

## <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Při vytváření skupin prostředků máte následující možnosti:

- Azure Portal
- Azure PowerShell
- Rozhraní příkazového řádku Azure
- Šablony
- Sady Azure SDK (.NET, Java atd.)

Pojďme si projít postup vytvoření skupiny prostředků na webu Azure Portal. Pokud chcete, můžete si ho zároveň vyzkoušet ve svém vlastním předplatném.

1. Otevřete webový prohlížeč a přihlaste se na web [Azure Portal](https://portal.azure.com/?azure-portal=true).

1. V levém okně vyberte **+ Vytvořit prostředek**.

1. Kliknutím na znaménko + přidáte novou skupinu prostředků. Do vyhledávacího pole zadejte **skupina prostředků** a stiskněte Enter.

1. Jako první položka v seznamu by se měl zobrazit prostředek ve skupině prostředků. Vyberte ho a pak klikněte na tlačítko **Vytvořit**.

    ![Výsledky hledání nové skupiny prostředků na webu Marketplace](../media/2-create-search-resource-group.png)

1. Zadejte název skupiny prostředků. V příkladu použijeme **msftvyuka-zakladni-infrastruktura-sp**. Vyberte pro skupinu předplatné a oblast, kde má být umístěna. Kliknutím na **Vytvořit** skupinu prostředků vytvořte.

    ![Vyplnění požadovaných polí k vytvoření skupiny prostředků](../media/2-create-resource-group.png)

Tak, to je celé. Vytvořili jste skupinu prostředků, kterou teď můžete použít při nasazování prostředků Azure. Pojďme se na skupinu prostředků, kterou jsme právě vytvořili, podívat podrobněji a připomenout si pár důležitých věcí, které je dobré vzít v úvahu.

## <a name="explore-a-resource-group-and-add-a-resource"></a>Prozkoumání skupiny prostředků a přidání prostředku

V levé nabídce na portálu vyberte **Skupina prostředků** a vyberte skupinu prostředků, kterou jste před chvílí vytvořili.

![Panel s přehledem skupiny prostředků](../media/2-rg-overview.png)

Na panelu Přehled se zobrazují základní informace o skupině prostředků: předplatné, ve kterém je umístěna, jeho ID, případné použité značky a historie nasazení do této skupiny prostředků. Značkami se budeme zabývat v další lekci. Odkaz na nasazení vás zavede na nový panel, kde uvidíte historii všech nasazení do této skupiny prostředků. Kdykoli vytvoříte prostředek, jde o nasazení, které se zobrazí v historii skupiny prostředků na tomto místě.

V horní části můžete přidávat další prostředky, měnit sloupce zobrazené v seznamu, přesunout skupinu prostředků do jiného předplatného nebo ji úplně odstranit.

V nabídce vlevo je k dispozici mnoho možností.

V této skupině prostředků zatím nemáme žádné prostředky, takže seznam v dolní části je prázdný. Pojďme ve skupině vytvořit několik prostředků.

1. Klikněte na tlačítko **+ Přidat** v horní části nebo na tlačítko **Vytvořit prostředky**. Obě fungují stejně. Zobrazí se panel s webem Marketplace.

1. Zadejte do vyhledávání **virtuální síť**. První výsledek by měl být prostředek virtuální sítě. Klikněte na něj a na další obrazovce klikněte na **Vytvořit**.

1. Virtuální síť pojmenujte **msftvyuka-vnet1**. V rozevíracím seznamu **Skupina prostředků** vyberte skupinu **msftvyuka-zakladni-infrastruktura**. U všech ostatních možností ponechte výchozí nastavení a klikněte na tlačítko **Vytvořit**.

1. Zopakujte celý postup ještě jednou a vytvořte druhou virtuální síť. Pojmenujte ji **msftvyuka-vnet2** a umístěte rovněž do skupiny prostředků **msftvyuka-zakladni-infrastruktura**.

1. Vraťte se do svojí skupiny prostředků. Na panelu **Přehled** byste měli vidět dvě virtuální sítě, které jste právě vytvořili.

    ![Panel Přehled skupiny prostředků s virtuálními sítěmi](../media/2-rg-with-vnet.png)

Naše skupina prostředků teď obsahuje dva prostředky virtuálních sítí, protože jsme při nasazení (tedy při vytváření prostředků) vždy zadali, do které skupiny prostředků chceme virtuální síť umístit. V této skupině prostředků bychom mohli vytvořit další prostředky, případně v rámci předplatného, kam prostředky nasazujeme, vytvořit další skupiny prostředků.

Při vytváření prostředků obvykle máte jako alternativu k použití existující skupiny možnost vytvořit novou skupinu prostředků. To celý proces trochu zjednodušuje, ale jak vidíte ve vaší nové organizaci, může to vést k tomu, že prostředky budou ve skupinách rozloženy bez důkladné úvahy, jak je uspořádat.

## <a name="use-resource-groups-for-organization"></a>Použití skupin prostředků v organizaci

Jak tedy skupiny prostředků ve vaší organizaci výhodně uplatníte? Tady je pár pokynů a osvědčených postupů, které vám při jejich uspořádání mohou pomoct.

### <a name="consistent-naming-convention"></a>Jednotné zásady vytváření názvů

Začít můžete používáním srozumitelných zásad pro vytváření názvů. Naši skupinu prostředků jsme pojmenovali **msftvyuka-zakladni-infrastruktura-sp**. Název ukazuje, k čemu se skupina používá (**msftvyuka**), jaké typy prostředků obsahuje (**zakladni-infrastruktura**) a jaký typ prostředku sama představuje (**sp**). Popisný název lépe vystihuje, co se ve skupině prostředků skrývá. Kdybychom ji nazvali **moje-skupina-prostredku** nebo **sp1**, na první pohled bychom nezískali žádnou představu, k čemu může sloužit. Z našeho názvu dokážeme odvodit, že skupina pravděpodobně obsahuje součásti základní infrastruktury. Pokud bychom vytvořili další virtuální sítě, účty úložišť nebo další prostředky, které společnost vnímá jako _základní infrastrukturu_, mohli bychom je sem umístit také a zpřehlednit tak uspořádání prostředků. Zásady vytváření názvů se můžou v různých společnostech – a dokonce i v různých odděleních – značně odlišovat, pomůže vám ale trocha plánování.

### <a name="organizing-principles"></a>Zásady dobré organizace

Skupiny prostředků se dají uspořádat nejrůznějšími způsoby. Ukažme si několik příkladů. V našem příkladu bychom do jedné skupiny mohli umístit všechny prostředky spadající do _základní infrastruktury_. Také bychom je ale mohli uspořádat striktně podle typu prostředku a umístit všechny virtuální sítě do jedné skupiny prostředků, všechny účty úložiště do druhé, instance služby Cosmos DB do třetí a tak dále.

![Obrázek prostředků uspořádaných podle typu](../media/2-resource-type-rg.png)

Prostředky bychom mohli uspořádat také podle prostředí (produkční, testovací, vývojové). V tomto případě by byly v jedné skupině všechny prostředky v produkčním prostředí, v další všechny testovací prostředky a tak dále.

![Obrázek prostředků uspořádaných podle prostředí](../media/2-environment-rg.png)

Mohli bychom použít i uspořádání podle oddělení (marketing, finance, lidské zdroje). Prostředky marketingu by tak náležely do jedné skupiny, prostředky financí do druhé a prostředky oddělení lidských zdrojů do třetí skupiny.

![Obrázek prostředků uspořádaných podle oddělení](../media/2-department-rg.png)

Tyto způsoby se dají i kombinovat, jako v tomto případě, kdy jsme prostředky uspořádali podle prostředí a oddělení. Jednu skupinu tvoří produkční prostředky finančního oddělení, další vývojářské prostředky téhož oddělení a obdobně jsou rozděleny prostředky patřící marketingu.

![Obrázek prostředků uspořádaných podle prostředí a oddělení](../media/2-env-dept-rg.png)

Při plánování strategie uspořádání prostředků je dobré vzít v úvahu několik faktorů: autorizaci, životní cyklus prostředků a fakturaci.

#### <a name="organizing-for-authorization"></a>Uspořádání s ohledem na autorizaci

Vzhledem k tomu, že skupiny prostředků představují obor řízení přístupu založeného na rolích, můžete prostředky uspořádat podle toho, _kdo_ je potřebuje spravovat. Pokud například za správu všech instancí Azure SQL Database zodpovídá tým správců databáze, usnadní mu práci umístění všech instancí do jedné skupiny prostředků. Na úrovni skupiny prostředků totiž týmu můžete udělit všechna oprávnění potřebná ke správě databází. Zároveň můžete správcům databáze odepřít přístup do skupiny prostředků s virtuálními sítěmi, aby nechtěně neprovedli změny u prostředků ležících mimo jejich zodpovědnost.

#### <a name="organizing-for-life-cycle"></a>Uspořádání s ohledem na životní cyklus

Už jsme zmínili, že skupiny prostředků slouží jako životní cyklus pro prostředky, které jsou v nich obsaženy, takže odstraněním skupiny se odstraní všechny prostředky v ní umístěné. Tento princip můžete výhodně využít zejména v oblastech, kde se prostředky vytvářejí na kratší doby, jako jsou neprodukční prostředí. Pokud nasazujete 10 serverů kvůli projektu, který bude trvat jen pár měsíců, usnadníte si vyčištění prostředků po jeho skončení, když všechny servery umístíte do jedné skupiny prostředků místo do deseti různých.

#### <a name="organizing-for-billing"></a>Uspořádání s ohledem na fakturaci

A konečně je umístění prostředků do jedné skupiny způsob, jak je seskupit pro účely sestav fakturace. Pokud se snažíte zorientovat v rozložení nákladů v podnikovém prostředí Azure, jejich seskupení podle skupin prostředků vám může poskytnout filtr a způsob řazení dat, s kterým si o nákladech uděláte přehled.

## <a name="summary"></a>Shrnutí

Celkově vzato platí, že při uspořádání prostředků do skupin máte značnou flexibilitu. Doporučujeme promyslet strategii a uplatňovat při práci se skupinami prostředků v prostředí Azure jednotný a srozumitelný přístup.

Probrali jsme některé hlavní zásady, které ve skupinách prostředků platí, postup jejich vytváření a přidávání prostředků a některé možnosti, jak je ve firemním prostředí využít k přehlednějšímu uspořádání prostředků.
