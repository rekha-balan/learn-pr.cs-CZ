Prošli jsme vaše prostředky a přesunuli je do skupin tak, aby byly přehledněji uspořádané. Ale co s prostředky, které mají několik využití? Jak si usnadnit jejich vyhledávání, filtrování a uspořádání? Pokud chcete uspořádání prostředků Azure ještě zlepšit, budou se vám hodit značky.

## <a name="what-are-tags"></a>Co jsou značky?

Značky jsou páry textových dat v podobě název/hodnota, které můžete přiřazovat prostředkům a skupinám prostředků. Značky umožňují přidat k prostředku vlastní podrobnosti navíc k těm, které jsou v Azure standardní, například:

- oddělení (finance, marketing atd.),
- prostředí (produkční, testovací, vývojové),
- nákladové středisko,
- životní cyklus a automatizace (např. vypínání a spouštění virtuálních počítačů).

 Každý prostředek může mít až 15 značek. Délka názvu je omezena na 512 znaků u všech typů prostředků s výjimkou účtů úložiště, které můžou mít maximálně 128 znaků. Hodnota značky je u všech typů prostředků omezena na 256 znaků. Značky z nadřazených prostředků se nedědí.

Značky lze přidávat a manipulovat s nimi prostřednictvím webu Azure Portal, rozhraní příkazového řádku Azure, Azure PowerShellu, šablon Resource Manageru a rozhraní REST API.

## <a name="apply-tags-to-resources"></a>Přidávání značek k prostředkům

Zkusme k prostředkům, které jsme vytvořili, přidat nějaké značky. Připomeňme si, že jsme vytvořili skupinu prostředků **msftvyuka-zakladni-infrastruktura-sp** a v ní dvě virtuální sítě, **msftvyuka-vnet1** a **msftvyuka-vnet2**. Názvy virtuálních sítí jsou poměrně obecné, rádi bychom je proto spojili se službami různých oddělení. 

1. Pusťte se s námi do toho a otevřete web [Azure Portal](https://portal.azure.com/?azure-portal=true). Přejděte do skupiny prostředků **msftvyuka-zakladni-infrastruktura-sp**.

1. Na kartě **Přehled** byste ve skupině prostředků měli vidět zmíněné dvě virtuální sítě. Ve výchozím zobrazení se sloupec se značkami nezobrazuje, pojďme ho tam tedy přidat. Nahoře vyberte **Upravit sloupce**. V seznamu **Dostupné sloupce** vyberte **Značky** a kliknutím na šipku **->** sloupec přidejte do seznamu **Vybrané sloupce**. Kliknutím na **Použít** změny uplatněte.

    ![Obrázek portálu, kde je vidět postup přidání sloupce Značky do zobrazení](../media/3-add-tag-column.PNG)

1. Teď byste sloupec Značky měli vidět, bude ale prázdný, protože jsme ještě žádné značky nepřidali. Přidáme je přímo tady. Značky můžete také přidat k prostředku, který to podporuje, na panelu **Značky** tohoto prostředku. V seznamu prostředků byste ve sloupci **ZNAČKY** měli vidět tužku, kterou můžete značky přímo upravit. Klikněte na ikonu tužky u prostředku **msftvyuka-vnet1**.

1. Zobrazíte tím dialogové okno, kde můžete značky upravovat. Přidejme k této virtuální síti pár značek. Do pole **NÁZEV** zadejte **Oddělení**a do pole **HODNOTA** zadejte **Finance**. Kliknutím na **Uložit** změny uložte a potom kliknutím na **Zavřít** zavřete dialogové okno.

    ![Obrázek portálu znázorňující dialogové okno pro přidání značek](../media/3-add-tag-1.PNG)

1. Stejné kroky teď zopakujeme u sítě **msftvyuka-vnet2**. U této virtuální sítě přidejte k prostředku značku **Oddělení:Marketing**.

    Zadané značky byste teď měli vidět u jednotlivých prostředků.

    ![Obrázek portálu znázorňující prostředky se značkami](../media/3-tags-displayed.PNG)

1. Další značky přidejme k oběma těmto prostředkům hromadně. Zaškrtněte u každé virtuální sítě políčko vlevo a klikněte na příkaz **Přiřadit značky** v horní nabídce. Když vyberete více prostředků, můžete k nim přidat značku hromadně. Usnadníte si práci v situacích, kdy chcete stejnou značku použít u několika prostředků.

    Přidejte k prostředkům značku **Prostředí:Školení**. V dialogovém okně byste měli vidět, že se značka přidala k oběma virtuálním sítím.

    ![Obrázek portálu znázorňující dialogové okno pro hromadné přidání značek](../media/3-add-bulk-tag.PNG)

    V seznamu prostředků pak uvidíte zobrazenou číslici **2**, protože teď máte u každého prostředku přiřazené dvě značky.

1. Pojďme se podívat, jak můžete značky využít k filtrování prostředků. V hlavní nabídce Azure na levé straně vyberte **Všechny prostředky**.

1. V rozevíracím seznamu **Všechny značky** v části **Prostředí** vyberte **Školení**. Měly by se zobrazit jen vaše dvě virtuální sítě, protože to jsou prostředky, které jste označili značkou **Prostředí:Školení**.

    ![Obrázek všech prostředků vyfiltrovaných pomocí značky Školení](../media/3-all-resources-tag-filter.PNG)

1. Tyto prostředky můžete dál filtrovat podle značek **Oddělení:Finance** nebo **Oddělení:Marketing**.

## <a name="use-tags-for-organization"></a>Použití značek k uspořádání

Výše uvedený příklad je jen jednou z možností, jak využít značky k uspořádání prostředků. Značky jsou flexibilní, a tak je můžete s výhodou použít různými způsoby.

Můžete je použít třeba k seskupení údajů o fakturaci. Pokud například používáte odlišný virtuální počítač pro každou organizační složku, můžete pomocí značek seskupit údaje o využití podle nákladových středisek. Značky lze použít také ke kategorizaci nákladů podle prostředí modulu spuštění, jako je například fakturované využití virtuálních počítačů běžících v produkčním prostředí. Když vyexportujete údaje o fakturaci nebo je prohlížíte přes rozhraní API pro fakturaci, jsou značky součástí těchto dat a dají se využít k dalším řezům dat z pohledu nákladů.

Opatřování prostředků značkami vám může pomoct při sledování zasažených prostředků. Systémy pro monitorování můžou údaje značek přidávat k výstrahám, takže přesně zjistíte, koho se událost dotkla. V našem příkladu jsme použili u prostředku **msftvyuka-vnet1** značku **Oddělení:Finance**. Kdyby se v síti **msftvyuka-vnet1** spustil alarm zahrnující tuto značku, věděli bychom, že stav, který alarm aktivoval, mohl postihnout finanční oddělení. Pokud dojde k problému, takové kontextové informace můžou být velmi cenné.

Značky se také běžně používají k automatizaci. Pokud chcete automatizovat vypínání a spouštění virtuálních počítačů ve vývojových prostředích v době mimo špičku, abyste ušetřili, můžou vám s tím pomoct právě značky. Přidejte k virtuálním počítačům značky **Vypnout:18:00** a **Spustit:7:00** a pak vytvořte úlohu automatizace, která bude tyto značky vyhledávat a podle hodnoty značky vypne nebo spustí počítače. V galerii runbooků služby Azure Automation najdete několik řešení, která značky podobným způsobem používají.

## <a name="summary"></a>Shrnutí

Značky jsou mimořádně flexibilní a umožňují k prostředkům přidat vlastní informace, které se dají využít mnoha různými způsoby. Můžou vám pomoct s uspořádáním prostředků, sledováním a přidělováním využití a vylepšením provozních možností v celé firmě.