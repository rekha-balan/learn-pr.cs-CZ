Microsoft nabízí bezplatné účty Azure DevOps jednotlivcům a malým týmům. Nabízí je také pro open-source projekty. Účty Azure DevOps si můžou zaregistrovat i velké organizace. Jejich týmy můžou mít tisíce členů. Zaregistrujeme si bezplatný účet Azure DevOps a podíváme se, jaké produkty můžeme v rámci DevOps používat.

## <a name="create-an-azure-devops-account"></a>Vytvoření účtu Azure DevOps

> [!NOTE]
> Pokud už máte účet Azure DevOps, můžete následující část přeskočit a rovnou přejít k **vytvoření nového projektu**.

K vytvoření účtu Azure DevOps použijte následující postup:

1. Otevřete okno prohlížeče a přejděte k webu Azure DevOps na adrese [https://dev.azure.com](https://dev.azure.com?azure-portal=true).

1. Klikněte na tlačítko **Začněte zdarma**.

1. K přihlášení použijte **účet Microsoft**. Pokud ho ještě nemáte, kliknutím **si ho vytvořte** a dokončete celý postup jeho vytvoření.

> [!NOTE]
> Je možné, že účet Microsoft už máte. Tyto účty většinou končí na hotmail.com nebo outlook.com.

1. Přečtěte si podmínky služby, prohlášení o ochraně osobních údajů a pravidla chování, a pokud s nimi souhlasíte, klikněte na **Pokračovat**.

Měl by se vytvořit nový účet Azure DevOps. 

> [!NOTE]
> Pokud si účet Azure DevOps vytvoříte sami, stanete se automaticky jeho vlastníky. Při rozhodování o názvu účtu postupujte uvážlivě, a nepoužívejte prosím názvy existujících právnických osob.

V seznamu organizací na levé straně stránky by se měl zobrazit název nové organizace. Když se v prohlížeči podíváte na adresní řádek, adresa vašeho účtu Azure DevOps bude `https://dev.azure.com/<Orgname>`. Pokud účet nemá přidružený název organizace, musíte vytvořit novou organizaci.

## <a name="create-a-new-organization"></a>Vytvoření nové organizace

1. V pravém podokně klikněte na tlačítko **Vytvořit novou organizaci**.

1. V okně s podmínkami služby Azure DevOps a oznámením o ochraně osobních údajů klikněte na **Pokračovat**.

1. Vedle pole **`dev.azure.com/`** vytvořte název organizace.

1. Klikněte na **Pokračovat**.

Tím vytvoříte pro své projekty Azure DevOps novou organizaci. Adresní řádek v prohlížeči bude mít formát `https://dev.azure.com/<Orgname>`.

## <a name="create-a-new-project"></a>Vytvoření nového projektu

V Azure DevOps jsou projekty organizace v podstatě synonymem pro aplikace. Název projektu by neměl obsahovat číslice. Vyhýbejte se názvům projektů jako například Mzdy 2.0 nebo Mzdy 2018. Lepší je pojmenovat projekt jednoduše **Mzdy*. Údaje 2.0 nebo 2018 budou pravděpodobně jenom větve v úložišti zdrojového kódu a iterace pracovních položek.

Po dokončení předchozího postupu a vytvoření nového účtu Azure DevOps by se měla zobrazit obrazovka pojmenovaná „Create a project to get started“ (Napřed vytvořte projekt). Pokud používáte stávající účet Azure DevOps, přejděte na adresu `https://dev.azure.com/<Orgname>`, kde najdete stejnou obrazovku.

1. Zadejte název prvního projektu. Jako název projektu použijeme **Contoso**.

1. Nechte viditelnost nastavenou na **Soukromé**. To znamená, že přístup k projektu budou mít jenom účty, které do něj přidáte. Možnost Veřejné bude vhodná pro open-source projekty, které budete vytvářet v budoucnosti.

1. Klikněte na tlačítko **Vytvořit projekt**.

Blahopřejeme! Zaregistrovali jste si nový účet Azure DevOps a vytvořili jste nový projekt. Tento účet můžete použít k získání dalších informací o Azure DevOps. Budete ho také potřebovat, pokud budete pokračovat ve výuce Azure DevOps v dalších modulech.

Zatím jsme na úplném začátku cesty k DevOps prostřednictvím služby Azure DevOps.