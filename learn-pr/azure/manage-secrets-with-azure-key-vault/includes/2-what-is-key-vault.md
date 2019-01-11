Azure Key Vault je *úložiště tajných kódů* – centralizovaná cloudová služba k ukládání tajných kódů aplikací. Key Vault vám pomáhá s kontrolou tajných kódů aplikací tím, že je uchovává v jednom centrálním umístění a poskytuje zabezpečený přístup, řízení oprávnění a protokolování přístupu.

Hlavní výhody používání služby Key Vault:

- Menší riziko náhodného vyzrazení tajného kódu díky zabezpečenému ukládání tajných kódů mimo konfiguraci a systém správy zdrojového kódu a díky eliminování možnosti kopírování tajných kódů prostřednictvím souborů a jejich vkládání do e-mailů a konverzací
- Omezený přístup k tajným kódům díky zásadám přístupu, které jsou přizpůsobené potřebám aplikací a jednotlivců
- Centralizované úložiště tajných kódů, aby mohlo více uživatelů a instancí aplikací přistupovat k hodnotám tajných kódů, které stačí aktualizovat na jediném místě
- Díky protokolování a monitorování přístupu snadno zjistíte, jak a kdy někdo zkoušel k tajným kódům získat přístup.

Tajné kódy se ukládají v jednotlivých trezorech (*vault*), což jsou prostředky Azure s vlastními konfiguracemi a zásadami zabezpečení, které můžete vytvořit pomocí kteréhokoli ze standardních nástrojů Azure pro správu, jako je Azure Portal nebo Azure CLI. Přístup k tajným kódům a správa trezoru se provádí prostřednictvím rozhraní REST API, což je také podporováno všemi nástroji Azure pro správu a také knihovnami klientů dostupnými pro celou řadu oblíbených jazyků. Každý trezor má jedinečnou adresu URL, na které je rozhraní API hostované.

> [!IMPORTANT]
> **Úložiště Key Vault je navržené k ukládání tajných konfiguračních kódů pro serverové aplikace.** Není určený k ukládání dat patřících uživatelům vaší aplikace a neměl by být používaný na klientské straně aplikace. To se odráží v jeho výkonových charakteristikách, rozhraní API a modelu nákladů.
>
> Uživatelská data by měla být uložená někde jinde, například v databázi Azure SQL s Transparentním šifrováním dat nebo v účtu úložiště s Šifrováním služby Storage. Tajné kódy používané vaší aplikací k přístupu k těmto úložištím dat můžou být uchovávané v úložišti Key Vault.

## <a name="what-is-a-secret-in-key-vault"></a>Co je tajný kód v úložišti Key Vault?

Tajný kód v úložišti Key Vault je dvojice řetězců název–hodnota. Názvy tajných kódů můžou mít délku 1–127 znaků, smí obsahovat jenom alfanumerické znaky a pomlčky a musí být v rámci trezoru jedinečné. Hodnotou tajného kódu může být libovolný řetězec UTF-8 o velikosti až 25 KB.

> [!TIP]
> Názvy tajných kódů samy o sobě nemusí být považované za tajné. Pokud si to implementace vyžaduje, můžete je uchovávat v konfiguraci vaší aplikace. Totéž platí pro názvy a adresy URL trezorů.

> [!NOTE]
> Key Vault podporuje kromě řetězců další dva druhy tajných kódů – *klíče* a *certifikáty* – a poskytuje užitečné funkce specifické pro jejich případy použití. Tento modul tyto funkce nepopisuje a soustředí se na tajné řetězce, jako jsou hesla a připojovací řetězce.
