K vytvoření účtu úložiště existuje několik nástrojů. Obvykle budete volit podle toho, jestli chcete grafické rozhraní (GUI) a jestli potřebujete automatizaci.

## <a name="available-tools"></a>Dostupné nástroje

Mezi dostupné nástroje patří:

- Azure Portal
- Azure CLI (rozhraní příkazového řádku)
- Azure PowerShell
- Klientské knihovny pro správu

Portál má grafické uživatelské rozhraní s vysvětlením jednotlivých nastavení. To usnadňuje použití portálu a pomáhá se získáváním informací o nastavení.

Všechny další nástroje ve výše uvedeném seznamu podporují automatizaci. Azure CLI a Azure PowerShell vám umožňují vytvářet skripty, zatímco knihovny pro správu vám umožňují začlenit výtvor do klientské aplikace.

## <a name="how-to-choose-a-tool"></a>Jak si nástroj zvolit

Účty úložiště jsou obvykle založené na analýze dat, takže bývají poměrně stabilní. V důsledku toho bývá obvykle vytváření účtu úložiště jednorázovou operací prováděnou na začátku projektu. Pro jednorázové aktivity je web Azure Portal nejběžnější volbou.

Ve výjimečných případech, kdy potřebujete automatizaci, si můžete vybrat mezi programovým rozhraním API nebo skriptem. Skripty obvykle rychleji vytvoříte a jsou méně náročné na správu, protože nepotřebují integrované vývojové prostředí (IDE), balíčky NuGet ani kroky sestavení. Pokud máte existující klientskou aplikaci, knihovny pro správu mohou být lákavou volbou. V opačném případě budou pravděpodobně vhodnější skripty.