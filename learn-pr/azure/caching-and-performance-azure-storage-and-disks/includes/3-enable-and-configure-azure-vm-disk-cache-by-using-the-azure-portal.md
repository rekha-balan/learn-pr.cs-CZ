Viděli jsme nastavení a vlastnosti, které můžete vybrat pro předpověď výkonu vašeho disku, teď se podíváme na způsoby, jak to vylepšit prostřednictvím _ukládání do mezipaměti_.

## <a name="disk-caching"></a>Mezipaměť disku

Mezipaměť je specializovaná součást, která ukládá data (obvykle do paměti) tak, aby se k nim mohlo rychleji přistupovat. Data v mezipaměti jsou často data, která se nedávno načetla nebo jsou výsledkem dřívějšího výpočtu. Cílem je, aby se k datům mohlo přistupovat rychleji, než když se načítají z disku.

Ukládání do mezipaměti používá specializované (a někdy nákladné) dočasné uložiště s rychlejším výkonem čtení a zápisu než úložiště trvalé. Vzhledem k tomu, že úložiště mezipaměti je často omezené, je nutné se rozhodnout, které operace s daty budou ukládání do mezipaměti využívat nejvíce. I tam, kde je mezipaměť široce dostupná, například v Azure, je důležité znát před vlastním rozhodnutím o typu ukládání do mezipaměti, které chcete použít, vzory zatížení jednotlivých disků.

**Mezipaměť pro čtení** se pokouší urychlit _načítání dat_. Místo čtení z trvalého úložiště se data načítají z rychlejší mezipaměti. Čtení dat může využít mezipaměť za následujících podmínek:

- Data se už načetla dříve a existují v mezipaměti.
- Mezipaměť je dostatečně velká, aby všechna tato data uchovala.

Je důležité si uvědomit, že ukládání do mezipaměti pro čtení je výhodné, pokud je fronta čtení _předvídatelná_, jako je sada sekvenčních čtení. U náhodných vstupně-výstupních operací, u kterých jsou data, ke kterým přistupujete, rozeseta po celém úložišti, nebude mít použití mezipaměti prakticky žádný význam, dokonce může výkon disku snížit.

**Mezipaměť pro zápis** se pokouší zrychlit _zápis dat_ do úložiště. Při použití mezipaměti pro zápis zvažuje aplikace data, která se mají uložit. Ve skutečnosti se data zařadí do fronty v mezipaměti a čekají na zápis na disk. Jistě si dovedete představit, že u tohoto mechanismu může dojít k chybě, například pokud se systém ukončí dříve, než se data v mezipaměti zapíšou. Některé systémy, jako je například SQL Server, se o zápis dat v mezipaměti na trvalý disk postarají samy.

### <a name="azure-disk-caching"></a>Mezipaměť disku v Azure

Existují dva typy ukládání do mezipaměti disku, které využívají úložný prostor na disku:

- Ukládání do mezipaměti Azure Storage
- Ukládání do mezipaměti disků virtuálních počítačů Azure

Ukládání do mezipaměti Azure Storage poskytuje služby mezipaměti pro Azure Blob Storage, Soubory Azure a další obsah v Azure. Konfigurace těchto typů mezipaměti není předmětem tohoto modulu.

Ukládání do mezipaměti disků virtuálních počítačů Azure je o optimalizace přístupu pro čtení a zápis k souborům virtuálních pevných disků připojených k virtuálním počítačům Azure. V tomto modulu se zaměříme na mezipaměť disku.

### <a name="azure-virtual-machine-disk-types"></a>Typy disků virtuálních počítačů Azure

S virtuálními počítači Azure se používají tři typy disků:

- **Disk s operačním systémem**: Když vytvoříte virtuální počítač Azure, Azure automaticky připojí virtuální pevný disk pro operační systém (OS).

- **Dočasný disk**: Když vytvoříte virtuální počítač Azure, Azure také automaticky přidá dočasný disk. Tento disk se používá pro data, jako jsou stránky a stránkovací soubory. Během údržby nebo opětovného nasazení virtuálního počítače může dojít ke ztrátě dat na tomto disku. Nepoužívejte ho k ukládání permanentních dat, jako jsou databázové soubory nebo transakční protokoly.

- **Datové disky**: Datový disk je virtuální pevný disk připojený k virtuálnímu počítači, aby ukládal data aplikací nebo jiná data, která potřebujete zachovat.

Disky s operačním systémem a datové disky využívají ukládání do mezipaměti disků virtuálních počítačů Azure. Velikost mezipaměti pro disk virtuálního počítače závisí na velikosti instance virtuálního počítače a na počtu disků, které jsou k virtuálnímu počítači připojené. Ukládání do mezipaměti lze povolit pro maximálně čtyři datové disky.

## <a name="cache-options-for-azure-vms"></a>Možnosti mezipaměti pro virtuální počítače Azure

Pro ukládání do mezipaměti disků virtuálních počítačů existují tři běžné možnosti:

- **Čtení/zápis** – Ukládání do mezipaměti se zpětným zápisem. Tuto možnost použijte, pouze pokud vaše aplikace v případě potřeby správně zpracovává zápis dat v mezipaměti na trvalé disky.
- **Jen pro čtení** – Čtení se provádí z mezipaměti.
- **Žádné** – Bez mezipaměti. Tuto možnost vyberte pro disky určené pouze pro zápis nebo s častým zápisem. Vhodné jsou soubory protokolů, protože se jedná o operace s častým zápisem.

Jednotlivé typy disků ale nemusí mít k dispozici všechny možnosti ukládání do mezipaměti. Následující tabulka obsahuje možnosti ukládání do mezipaměti pro jednotlivé typy disků:

|               | **Jen pro čtení**  | **Čtení/zápis** | **Žádné** |
|---------------|----------------|----------------|----------|
| Disk OS       | ano            | ano (výchozí)  | ano      |
| Datový disk     | ano (výchozí)  | ano            | ano      |
| Dočasný disk     | ne             | ne             | ne       |

> [!NOTE]
> U virtuálních počítačů **L-Series** a **B-Series** nejde možnosti mezipaměti disku změnit.

## <a name="performance-considerations-for-azure-vm-disk-caching"></a>Důležité informace týkající se ukládání do mezipaměti disků virtuálních počítačů Azure

Jakým způsobem mohou nastavení mezipaměti ovlivnit výkon úloh spuštěných na virtuálních počítačích Azure?

### <a name="os-disk"></a>Disk OS

Výchozí chování disku OS virtuálního počítače je nastaveno na používání mezipaměti v režimu čtení/zápisu. Pokud máte aplikace, které ukládají datové soubory na disk OS, a tyto aplikace provádějí velké množství operací čtení datových souborů či zápisu do nich, zvažte přesunutí těchto souborů na datový disk s vypnutou mezipamětí. Proč to tak je? Pokud fronta čtení neobsahuje sekvenční čtení, ukládání do mezipaměti nebude mít prakticky žádný význam. Režie na údržbu mezipaměti (jako by data byla sekvenční) může snížit výkon disku.

### <a name="data-disks"></a>Datové disky

U aplikací citlivých na výkon byste měli používat datové disky, a nikoli disky OS. Použití samostatných disků vám umožní nakonfigurovat odpovídající nastavení mezipaměti pro každý disk.

Když například na virtuálních počítačích Azure s SQL Serverem povolíte u datových disků ukládání do mezipaměti **jen pro čtení** (pro standardní data a data TempDB), může se výrazně zlepšit výkon. Naopak soubory protokolů je vhodné ukládat na datové disky bez mezipaměti.

> [!WARNING]
> Při změně nastavení mezipaměti disku Azure se cílový disk odpojí a pak znovu připojí. Pokud jde o disk s operačním systémem, restartuje se celý virtuální počítač. Před změnou nastavení mezipaměti disku zastavte všechny aplikace nebo služby, které by mohly být tímto přerušením ovlivněny.

Nastavení mezipaměti disku virtuálního počítače můžete nakonfigurovat pomocí některého z následujících nástrojů:

- Azure Portal
- Azure CLI
- Azure PowerShell
- Šablony Resource Manageru

## <a name="using-the-azure-portal-to-configure-caching"></a>Konfigurace ukládání do mezipaměti pomocí webu Azure Portal

Když zřizujete nový virtuální počítač pomocí webu Azure Portal, nemůžete změnit výchozí konfiguraci mezipaměti disku OS (čtení/zápis), dokud nebude virtuální počítač nasazený.

Když přidáváte datový disk k existujícímu virtuálnímu počítači, můžete nakonfigurovat možnost mezipaměti před nasazením disku na virtuální počítač.

Při změně nastavení mezipaměti disku Azure se cílový disk odpojí a znovu připojí. Pokud jde o disk s operačním systémem, restartuje se celý virtuální počítač. Před změnou nastavení mezipaměti disku zastavte všechny aplikace nebo služby, které by mohly být tímto přerušením ovlivněny.

Pojďme vytvořit virtuální počítač a změnit nastavení mezipaměti pomocí webu Azure Portal.
