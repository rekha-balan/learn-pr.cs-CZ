## <a name="creating-key-vaults-for-your-applications"></a>Vytváření trezorů klíčů pro aplikace

Doporučuje se vytvořit každé aplikaci samostatný trezor pro každé prostředí nasazení, jako je vývoj, testování a produkční prostředí. Ke sdílení tajných kódů mezi aplikacemi je možné použít trezory, ale s rostoucím počtem kódů v trezoru se zvyšuje také dopad úspěšného útoku a získání přístupu ke čtení.

> [!TIP]
> Pokud používáte stejné názvy tajných kódů pro aplikaci v různých prostředích, stačí při přechodu do jiného prostředí změnit jediný parametr, a to adresu URL trezoru.

Vytvoření trezoru nevyžaduje žádnou počáteční konfiguraci. Vaší identitě uživatele je automaticky udělena úplná sada oprávnění pro správu tajných kódů a můžete začít okamžitě přidávat tajné kódy. Jakmile máte trezor, přidávání a správa tajných kódů se dá dělat v jakémkoli rozhraní Azure pro správu, včetně webu Azure Portal, Azure CLI a Azure PowerShellu. Při nastavování vaší aplikace pro použití trezoru je potřeba přiřadit správná oprávnění, jak uvidíme v další jednotce.

## <a name="vault-authentication-and-permissions"></a>Ověřování a uživatelská oprávnění k trezoru

Rozhraní API Azure Key Vault používá k ověřování uživatelů a aplikací službu Azure Active Directory. Zásady přístupu k trezoru jsou založeny na *akcích* a platí pro celý trezor. Například aplikace s oprávněními **Get** (čtení tajných kódů), **List** (získání názvů všech tajných kódů), a **Set** (vytváření nebo aktualizace hodnot tajných kódů) může tajné kódy vytvářet, získávat jejich seznam a číst a nastavovat hodnoty všech tajných kódů v trezoru.

*Všechny* akce prováděné s trezorem vyžadují oprávnění a autorizaci &mdash; neexistuje způsob, jak umožnit jakýkoli typ anonymního přístupu.

> [!TIP]
> Při udělování přístupu do trezoru pro vývojáře a aplikace udělujte jenom minimální potřebná oprávnění. Omezení oprávnění pomáhají zabránit zneužití náhodných chyb kódu a omezují dopad krádeže přihlašovacích údajů nebo škodlivého kódu vloženého do aplikace.

Vývojáři obvykle potřebují jen oprávnění **Get** a **List** k trezoru vývojového prostředí. Vedoucí týmu nebo zkušený vývojář bude potřebovat úplná oprávnění k trezoru, aby mohl měnit a přidávat tajné kódy podle potřeby. Úplná oprávnění k trezorům provozního prostředí jsou obvykle vyhrazena pro vedoucí provozní personál.

Pro aplikace často stačí oprávnění **Get**. Některé aplikace mohou vyžadovat také oprávnění **List**, podle způsobu jejich implementace. Aplikace, kterou budeme v tomto cvičení do našeho modulu implementovat, vyžaduje oprávnění **List** kvůli používané technice čtení tajných kódů z trezoru.

Vzhledem ke všem dosavadním potížím společnosti s tajnými kódy aplikací vás vedení požádalo o vytvoření malé úvodní aplikace, která by správně nasměrovala ostatní vývojáře. Aplikace má využívat osvědčené postupy správy tajných kódů tím nejjednodušším a nejbezpečnějším způsobem.

## <a name="create-the-vault-and-store-the-secret-in-it"></a>Vytvoření trezoru pro uložení tajného kódu

Začneme tím, že vytvoříme trezor a uložíme do něj tajný kód.

### <a name="create-the-vault"></a>Vytvoření trezoru

Napřed vytvoříme trezor a uložíme do něj tajný kód.

[!include[](../../../includes/azure-sandbox-activate.md)]

**Názvy služby Key Vault musí být globálně jedinečné, takže budete muset zvolit jedinečný název**. Názvy trezorů můžou mít 3–24 znaků a smí obsahovat jenom písmena, číslice a pomlčky. Název trezoru si poznamenejte, protože ho budete v tomto cvičení používat.

[!include[](../../../includes/azure-sandbox-regions-first-mention-note.md)]

Vytvořte trezor spuštěním následujícího příkazu v Cloud Shellu.

```azurecli
az keyvault create \
    --resource-group <rgn>[sandbox resource group name]</rgn> \
    --name <your-unique-vault-name>
```

Po dokončení se zobrazí výstup JSON s popisem nového trezoru.

> [!TIP]
> Příkaz použil předem vytvořenou skupinu prostředků pojmenovanou **<rgn>[Skupina prostředků v sandboxu]</rgn>**. Pokud pracujete s vlastním předplatným, budete si asi chtít vytvořit novou skupinu prostředků nebo použít stávající skupinu, kterou máte vytvořenou.

### <a name="add-the-secret"></a>Přidání tajného kódu

Teď přidejte tajný kód: náš tajný kód bude mít název **SecretPassword** a hodnotu **reindeer_flotilla**.

```azurecli
az keyvault secret set \
    --name SecretPassword \
    --value reindeer_flotilla \
    --vault-name <your-unique-vault-name>
```

Brzy napíšeme kód naší aplikace, ale napřed si musíme vysvětlit, jak se tato aplikace bude přihlašovat k trezoru.