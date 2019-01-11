Paměť je nejdůležitějším prostředkem pro Azure Cache for Redis, protože se jedná o databázi v paměti. Když začnete přidávat data překračující dostupné místo v paměti, můžete narazit na problémy. Azure Cache for Redis podporuje zásady vyřazení, které určují, jak by měla být data zpracována při nedostatku místa.

Zde nastavíte zásadu vyřazení, která určuje, co se má s daty stát, když překročíte maximální množství místa v paměti.

## <a name="what-is-an-eviction-policy"></a>Co je zásada vyřazení?

Zásada vyřazení je plán, který určuje, jak by se měla data zpracovat, když překročíte maximální množství místa v paměti. Pomocí zásady vyřazení můžete například určit, aby služba Azure Cache for Redis odstranila náhodný klíč a uvolnila tak místo pro vložení nových dat.

### <a name="types-of-eviction-policies"></a>Typy zásad vyřazení

Azure Cache for Redis nabízí šest různých zásad vyřazení. Všechny uvedené hodnoty při pokusu o vložení dat při nedostatku místa v paměti provedou nějakou akci.

* **noeviction:** Žádná zásada vyřazení. Vrátí chybovou zprávu, pokud se pokusíte vložit data.

* **allkeys-lru:** Odebere nejdéle nepoužitý klíč.

* **allkeys-random:** Odebere náhodný klíč.

* **volatile-lru:** Odebere nejdéle nepoužitý klíč ze všech klíčů s nastaveným vypršením platnosti.

* **volatile-ttl:** Odebere klíč s nejkratší životností na základě nastaveného vypršení platnosti.

* **volatile-random:** Odebere náhodný klíč, který má nastavené vypršení platnosti.

## <a name="how-to-set-an-eviction-policy"></a>Nastavení zásady vyřazení

Azure pro nastavení zásady vyřazení pro Azure Cache for Redis nabízí jednoduchou rozevírací nabídku. Vyberte **Upřesnit nastavení** a použijte rozevírací nabídku **maxmemory-policy**.

Paměť je pro Azure Cache for Redis zásadní, a proto je k dispozici podpora pro zásady vyřazení. Zásada vyřazení určuje, co se má stát s existujícími daty, když nemáte dostatek místa v paměti a pokoušíte se vložit nová data.