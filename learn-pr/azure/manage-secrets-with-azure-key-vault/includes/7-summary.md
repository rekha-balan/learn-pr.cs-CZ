V tomto modulu jste ve službě Azure Key Vault zabezpečili konfiguraci tajných kódů aplikace. V trezoru proběhne pomocí spravované identity ověření kódu aplikace a tajné kódy z trezoru se při spuštění automaticky načtou do paměti.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

Pokud chcete vyčistit úložiště Cloud Shell, odstraňte adresář `KeyVaultDemoApp`.

## <a name="next-steps"></a>Další kroky

Kdyby to byla opravdová aplikace, co by se dělo dál?

- Všechny tajné kódy aplikace dávejte do svých trezorů. Už nemáte důvod mít je v konfiguračních souborech.
- Pokračujte ve vývoji aplikace. Vaše produkční prostředí je zcela nastavené, takže u budoucích nasazení už všechny kroky nastavení opakovat nemusíte.
- Podpořte vývoj vytvořením trezoru development-environment, který bude obsahovat tajné kódy se stejnými názvy, ale jinými hodnotami. Udělte vývojovému týmu oprávnění a v konfiguračním souboru vývojového prostředí aplikace nakonfigurujte název trezoru. Konfigurace závisí na implementaci: u ASP.NET Core `AddAzureKeyVault` automaticky rozpozná místní instalace sady Visual Studio a Azure CLI a použije přihlašovací údaje Azure nakonfigurované v těchto aplikacích k přihlášení a získání přístupu k trezoru. U Node.js můžete vytvořit instanční objekt vývojového prostředí s oprávněními k trezoru a aplikaci nechat ověřit pomocí `loginWithServicePrincipalSecret`.
- Vytvořte další prostředí pro účely, jako je testování přijetí u zákazníků.
- Rozdělte trezory mezi různá předplatná a/nebo skupiny prostředků, abyste je izolovali.
- Příslušným osobám udělte přístup do trezorů jiných prostředí.

## <a name="further-reading"></a>Další materiály

- [Dokumentace ke službě Key Vault](https://docs.microsoft.com/azure/key-vault/)
- [Další informace o příkazu AddAzureKeyVault a jeho pokročilých možnostech](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration?view=aspnetcore-2.1&tabs=aspnetcore2x)
- [Tento kurz](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application) vás provede použitím příkazu `KeyVaultClient`, včetně jeho ručního ověření v Azure Active Directory pomocí tajného klíče klienta místo spravované identity.
- [Dokumentace ke službě tokenů spravovaných identit pro prostředky Azure](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity#using-the-rest-protocol) pro samostatnou implementaci pracovního postupu ověřování.