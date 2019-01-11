Azure Key Vault používá k ověřování uživatelů a aplikací pokoušejících se o získání přístupu k trezoru službu **Azure Active Directory**. Abychom naší webové aplikaci mohli udělit přístup k trezoru, musíme ji nejprve zaregistrovat do služby Azure Active Directory. Registrací se pro aplikaci vytvoří identita. Jakmile má aplikace identitu, můžeme jí přiřadit oprávnění k trezoru.

Aplikace a uživatelé se ve službě Key Vault ověřují pomocí ověřovacího tokenu Azure Active Directory. K získání tokenu ze služby Azure Active Directory se vyžaduje tajný kód nebo certifikát, protože jinak by jakýkoli držitel tokenu mohl použít identitu aplikace k získání přístupu ke všem tajným kódům v trezoru.

Naše tajné kódy aplikace jsou bezpečně uložené v trezoru, ale pro přístup k nim stále potřebujeme tajný kód nebo certifikát uložený mimo trezor. Tento problém se označuje jako *problém se zaváděním* a Azure pro něj má řešení.

## <a name="managed-identities-for-azure-resources"></a>Spravované identity pro prostředky Azure

Spravované identity pro prostředky Azure je funkce Azure, která vaší aplikaci umožní přistupovat ke službě Key Vault a dalším službám Azure bez nutnosti správy tajných kódů mimo trezor. Použití spravované identity je jednoduchým a bezpečným způsobem, jak využít službu Key Vault z webové aplikace.

Když povolíte spravovanou identitu ve webové aplikaci, Azure jenom pro vaši aplikaci aktivuje samostatnou službu REST pro udělování tokenů. Vaše aplikace bude vyžadovat tokeny z této služby, místo aby je vyžadovala přímo z Azure Active Directory. Pro přístup k této službě potřebuje aplikace tajný kód. Tento tajný kód se ale při spuštění vloží do proměnných prostředí vaší aplikace službou App Service. Hodnotu tohoto tajného kódu není potřeba spravovat ani nikam ukládat. Tajný kód ani koncový bod služby tokenů spravované identity navíc nejsou mimo vaši aplikaci přístupné.

Funkce Spravované identity pro prostředky Azure také pro vás zaregistruje vaši aplikaci v Azure Active Directory a tuto registraci odstraní, pokud odstraníte webovou aplikaci nebo zakážete její spravovanou identitu.

Spravované identity jsou k dispozici ve všech edicích Azure Active Directory, včetně edice Free, která je součástí předplatného Azure. Použití spravovaných entit ve službě App Service je bez dalších poplatků a nevyžaduje žádnou konfiguraci a dají se pro aplikaci kdykoliv povolit nebo zakázat.

> [!NOTE]
> Funkce Spravované identity pro prostředky Azure se v současné době nepodporuje pro webové aplikace v Linuxu ani webové aplikace typu kontejner.

K povolení spravované identity pro webovou aplikaci stačí jediný příkaz Azure CLI bez jakékoliv konfigurace. Provedeme to později, když nastavíme aplikaci App Service a nasadíme ji do Azure. Předtím však využijeme naše znalosti o spravovaných identitách a napíšeme kód naší aplikace.