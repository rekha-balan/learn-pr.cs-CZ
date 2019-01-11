Ukázali jsme si, jak zajistit dostupnost a obnovitelnost vaší aplikace vytvořením návrhu podporujícím vysokou dostupnost, zotavení po havárii a zálohování. Pojďme se podívat na některé z klíčových pojmů.

## <a name="high-availability"></a>Vysoká dostupnost

- Vysoká dostupnost je schopnost zvládat ztrátu nebo výrazné snížení výkonu některé komponenty systému.
- Zhodnoťte zajištění vysoké dostupnosti aplikace zaměřením na tři klíčové oblasti:
  - Definovaná smlouva SLA.
  - Možnosti zajištění vysoké dostupnosti vaší aplikace.
  - Možností zajištění vysoké dostupnosti závislých aplikací.
- K zajištění vysoké dostupnosti pro úlohy virtuálního počítače použijte skupiny dostupnosti a zóny dostupnosti v Azure.
- Pomocí služeb pro vyrovnávání zatížení, jako jsou Azure Traffic Manager, Azure Application Gateway a Azure Load Balancer, distribuujte zatížení mezi dostupnými systémy.
- Služby PaaS mají integrovanou vysokou dostupnost, proto využívejte tyto služby ve vaší architektuře, kde je to možné.

## <a name="disaster-recovery"></a>Zotavení po havárii

- Při zotavení po havárii jde o *zotavení z událostí s vysokým dopadem*, které způsobují výpadky a ztrátu dat.
- Vytvořte plán zotavení po havárii a definujte v něm postupy, zodpovědnost a aktivity, které jsou potřebné k zotavení po havárii.
- Definujte pro vaši aplikaci RPO a RTO, které vám pomohou určit požadavky na zotavení po havárii pro vaši aplikaci.
- Pomocí zálohování a replikace zajistěte kopie vašich systémů a dat pro obnovení.
- Použijte službu Azure Site Recovery k zajištění procesu obnovení vaší aplikace v rámci zotavení po havárii.
- Otestujte svůj plán zotavení po havárii, abyste identifikovali nedostatky a posoudili relevanci kroků.

## <a name="backup-and-restore"></a>Zálohování a obnovení

- Využívejte zálohy k obnovení dat jako součást vaší strategie zotavení po havárii nebo pro scénáře ztráty izolovaných dat.
- Pomocí služby Azure Backup vytvořte úplnou zálohu virtuálního počítače, zálohu souborů a složek a zálohu systémů v místním prostředí.
- Služby PaaS, například Azure SQL Database a Azure App Service, mají často integrované možnosti zálohování. Využijte tyto možnosti zálohování, pokud je to možné, seznamte se s jejich výchozí konfigurací i s celkovými možnosti, které nabízejí.
- Pravidelně testujte své postupy pro obnovení, abyste zajistili, že jsou platné a postačující.
