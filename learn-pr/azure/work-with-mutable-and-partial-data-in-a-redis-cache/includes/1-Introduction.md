Představte si, že vytváříte mobilní aplikaci pro rychlé zprávy. Vaše aplikace umožňuje uživatelům odesílat zprávy všem členům specifické skupiny definované uživateli. O každém uživateli je nutné uložit nějaká data, například jejich uživatelské jméno, e-mail a heslo. Proto se rozhodnete použít SQL Server k vytvoření relační databáze pro tyto informace. Samotné zprávy však musí být odesílány a používány rychle a relační databáze je na to příliš pomalá.

Kvůli nabízeným výhodám se rozhodnete vytvořit instanci Azure Cache for Redis. Z modulu **Optimalizace webových aplikací pomocí ukládání dat jen pro čtení do mezipaměti s využitím Redis** si vybavíte, že Redis Cache je úložiště datových struktur v paměti, které je možné využívat jako databázi, mezipaměť nebo zprostředkovatele zpráv.

Při použití Azure Cache for Redis byste mohli pomocí transakcí zajistit, aby se zprávy s obrázky a textem odesílaly dohromady. Použijte vypršení platnosti dat po hodině k resetování názvu skupinového chatu. Nakonec použijte zásady vyřazení pro zajištění toho, aby se při nedostatku místa nejprve odstranily nejstarší zprávy.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Seskupení více operací do transakce
- Nastavení doby vypršení platnosti pro data
- Správa nedostatku paměti
- Použití modelu doplňování do mezipaměti
- Použít balíčku ServiceStack.Redis v konzolové aplikaci .NET Core