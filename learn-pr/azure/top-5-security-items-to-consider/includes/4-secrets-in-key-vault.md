Tajemství přestává být tajemstvím, pokud o něm všichni ví. Jestli ukládáte důvěrné údaje, jako jsou připojovací řetězce, tokeny zabezpečení, certifikáty a hesla, přímo do kódu, můžete rovnou ostatním nabídnout, aby je použili k jiným účelům, než jsou určené. Vhodné není ani uložení těchto dat v konfiguraci webu, protože tím v podstatě komukoli, kdo má přístup ke zdrojovému kódu nebo webovému serveru, dáváte přístup ke svým soukromým datům.

K uložení těchto tajných údajů byste měli vždy použít **Azure Key Vault**.

## <a name="what-is-azure-key-vault"></a>Co je Azure Key Vault?
Azure Key Vault je *úložiště tajných kódů*. Tato centralizovaná cloudová služba se používá k ukládání tajných kódů aplikací. Ve službě Key Vault jsou vaše tajné údaje v bezpečí. Tajné kódy aplikací, které umožňují zabezpečený přístup, řízení oprávnění a protokolování přístupu, jsou uchovávané na jednom centrálním místě.

Tajné kódy jsou uložené v jednotlivých *trezorech*. Každý trezor má vlastní konfiguraci a zásady zabezpečení, kterými se řídí přístup. Přístup k datům máte prostřednictvím rozhraní REST API nebo sady SDK klienta, která je k dispozici ve většině jazyků.

> [!IMPORTANT]
> **Úložiště Key Vault je navržené tak, abyste do něj mohli ukládat tajné konfigurační kódy serverových aplikací.** Není určený k ukládání dat patřících uživatelům vaší aplikace a neměl by být používaný na klientské straně aplikace. To se odráží v jeho výkonových charakteristikách, rozhraní API a modelu nákladů.
>
> Uživatelská data by měla být uložená někde jinde, například v databázi Azure SQL s Transparentním šifrováním dat nebo v účtu úložiště s Šifrováním služby Storage. V Key Vaultu můžou být uložené tajné kódy používané vaší aplikací pro přístup k těmto úložištím dat.

## <a name="why-use-a-key-vault-for-my-secrets"></a>Proč používat pro tajné kódy Key Valut

Když budete spravovat klíče a ukládat tajné kódy ručně, může to být složité a může docházet k chybám. Při ruční obměně certifikátů se může stát, že se bez nich budete muset několik hodin nebo dnů obejít. Řekli jsme si, že při uložení připojovacích řetězců v konfiguračním souboru nebo úložišti kódu hrozí odcizení vašich přihlašovacích údajů.

Do Key Vaultu mohou uživatelé ukládat připojovací řetězce, tajné kódy, hesla, certifikáty, zásady přístupu, zámky souborů (pro položky v Azure určené jen ke čtení) a automatické skripty.  Key Vault také protokoluje přístup a aktivity, umožňuje monitorovat řízení přístupu (IAM) v rámci předplatného a nabízí diagnostické nástroje a také nástroje na měření, upozorňování a řešení potíží, které zajišťují potřebný přístup.

Další informace o používání služby Azure Key Vault ve [Správě tajných kódů pro serverové aplikace pomocí služby Azure Key Vault](../../manage-secrets-with-azure-key-vault/index.yml).

## <a name="summary"></a>Shrnutí

Pokud ke správě tajných kódů používáte Azure Key Vault, nemusíte si dělat starosti s odcizením přihlašovacích údajů, ruční obměnou klíčů ani s obnovováním certifikátů.
