V závěrečné lekci tohoto modulu si povíme o tom, kam dál, pokud vás zajímá další objevování SRE. 

## <a name="reading-and-watching"></a>Čtení a sledování

Nejlepším zdrojem podrobných informací o SRE je trojice knih, vydaných na toto téma

1. [_Site Reliability Engineering: How Google Runs Production Systems_](http://shop.oreilly.com/product/0636920041528.do) (Site Reliability Engineering: Jak Google provozuje produkční systémy, známý také jako „The SRE Book“)
1. [_The Site Reliability Workbook: Practical Ways to Implement SRE_](http://shop.oreilly.com/product/0636920132448.do) (Pracovní sešit SRE: Praktické způsoby implementace SRE, známá také jako „The SRE Workbook“)
1. [_Seeking SRE: Conversations About Running Production Systems at Scale_](http://shop.oreilly.com/product/0636920063964.do) (Hledání SRE: Rozhovory o provozování produkčních systémů ve velkém)

(Upozornění: Hlavní autor tohoto modulu je zároveň editorem třetí knihy.)

Každá z těchto knih přináší důležitou sadu informací:

- SRE Book podrobně vysvětluje, jak společnost Google v průběhu let implementovala SRE.

- SRE Workbook jako doplněk SRE Book poskytuje podrobnější odpovědi nejen na otázku „Co?“ ohledně SRE v Googlu a na několika dalších pracovištích, ale i na otázky „Jak?“ a „Proč?“.

- Hledání SRE nabízí hlubší pohled do světa SRE, včetně informací o tom, jak bylo implementováno do jiných prostředí.

Nezapomeňte ke všem třem knihám přistupovat kriticky a s odstupem. Zdaleka ne všechno, co v nich najdete, můžete snadno aplikovat na sebe nebo na svou organizaci. Nějakou dobu trvá, než poznáte, které informace jsou pro vás přínosné. Zamyslete se, která část vaší firemní kultury a hodnot již v zásadě je v souladu s činnostmi SRE tak, jak jsme je popsali, a v jakých oblastech by byla implementace náročnější.

Pokud jste více vizuálně zaměření, zkuste se podívat na přednášku Bena Treynora [Keys to SRE (Klíče k SRE)](https://www.usenix.org/conference/srecon14/technical-sessions/presentation/keys-sre) z konference SREcon14. Treynor poskytuje velmi přesvědčivé vysvětlení toho, v čem podle něj SRE spočívá (alespoň v rámci Googlu). Velmi užitečné mohou být také další záznamy přednášek o SRE nejen z [těchto konferencí](https://www.usenix.org/conferences/byname/925).

## <a name="talk-to-other-interested-people"></a>Komunikujte s ostatními zainteresovanými lidmi

Jakkoli je čtení o SRE důležité, diskuze s kolegy na toto téma může být často mnohem přínosnější. Diskutování o vašich problémech, úspěších i neúspěších na poli SRE může být klíčové k porozumění nejjemnějším nuancím tohoto tématu. 

Existuje mnoho setkání a konferencí, které se tématem SRE zabývají. Asi nejvíce k tématu jsou [konference SREcon](https://www.usenix.org/conferences/byname/925) pořádané společností USENIX (prohlášení: Hlavní autor tohoto modulu je jedním ze spoluzakladatelů konferencí SREcon).

SRE se postupně stává námětem přednášek na dalších konferencích, například [Velocity](https://conferences.oreilly.com/velocity), [LISA](https://www.usenix.org/conferences/byname/5), a místních konferencích DevOps, jako jsou [DevOps Days (Dny DevOps)](https://www.devopsdays.org). Hledejte SRE a ty, kteří se jím zabývají, kdekoli jen můžete.

## <a name="first-steps-at-work"></a>První kroky v práci

Pokud chcete začít zjišťovat, jak by to vypadalo, kdybyste SRE přivedli do vašeho prostředí, mějte na paměti, že SRE neznamená „všechno nebo nic“.  Můžete začít přijímat zásady a postupy SRE po krůčcích.

Mikey Dickerson, SRE všeobecně uznávaný za svou práci v tom, z čeho později vznikla United States Digital Service (mimo jiné odpovědná za záchranu healthcare.gov), navrhl hierarchii spolehlivosti jako poctu Maslowově hierarchii potřeb. Je citovaná v [části Postupy](https://landing.google.com/sre/book/chapters/part3.html) první knihy SRE.

Tato hierarchie navrhuje, že nejprve je nutné zajistit funkční a důvěryhodný monitoring vašeho prostředí. To je také nutný první krok k SRE ve vašem prostředí. Nemůžete říct, jestli je něco spolehlivé (jestli se to zlepšuje nebo zhoršuje), když to nedokážete změřit.

Jakmile máte monitorovací platformu, které můžete důvěřovat, dalším dosažitelným krokem je vybrat fungující službu a začít diskutovat o SLI a SLO pro tuto službu. Začněte jednoduše. Vytvořte pro ni ukazatele SLI a cíle SLO, implementujte je do monitorovacího systému a pozorujte, co se stane, když začnete sledovat spolehlivost optikou SRE. Je to skvělý způsob, jak začít.
