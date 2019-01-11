V tomto modulu jste definovali, co znamená Azure Resource Manager a co obsahuje šablona Resource Manageru.

Šablony Resource Manageru jsou _deklarativní_. To znamená, že definujete, _jaké_ prostředky potřebujete, a podrobnosti nasazení necháte na Resource Manageru.

K nasazení virtuálního počítače jste použili existující šablonu Azure pro rychlý start. Šablony pro rychlý start představují skvělý způsob, jak začít s nasazováním. Předvádějí rovněž doporučené postupy, ze kterých se při vytváření vlastních šablon můžete přiučit.

Šablonu pro rychlý start jste také rozšířili o konfiguraci softwaru webového serveru na virtuálním počítači. Rozšíření existující šablony představuje skvělý způsob, jak něco rychle zprovoznit a pochopit, jak do sebe jednotlivé části zapadají.

Šablony Resource Manageru se rovněž dají _skládat_. Při sestavování svých nasazení můžete vytvořit menší šablony, které definují určitou část systému, a jejich zkombinováním pak vytvořit kompletní systém.

Myslete na analytiky, kterým ve firmě poskytující finanční služby zajišťujete podporu. Při budování knihovny šablon Resource Manageru budete moci sestavit nasazení pro jednotlivé analytiky mnohem rychleji. Po dokončení všech finančních modelů můžete jediným příkazem Azure CLI celé nasazení zlikvidovat.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="learn-more"></a>Další informace

Nejlepším učením je praxe. Zkuste vytvořit šablonu Resource Manageru, která automatizuje jedno z vašich nasazení.

Přečtěte si [dokumentaci k Resource Manageru v Azure](https://docs.microsoft.com/azure/azure-resource-manager?azure-portal=true) a zjistěte, jak vytvářet, nasazovat, spravovat, auditovat a řešit problémy se šablonami.

Zde je několik materiálů, které podrobněji vysvětlují vše, co jste se v tomto modulu naučili.

* [Šablony Azure pro rychlý start](https://azure.microsoft.com/resources/templates?azure-portal=true)
* [Vizualizér Azure Resource Manageru](http://armviz.io?azure-portal=true)
* [Nasazení jednoduchého virtuálního počítače s Windows](https://azure.microsoft.com/resources/templates/101-vm-simple-windows?azure-portal=true)
* [Nasazení jednoduchého virtuálního počítače s Ubuntu Linuxem](https://azure.microsoft.com/resources/templates/101-vm-simple-linux?azure-portal=true)

Zde je několik témat, která jsme zde neprobírali, ale o která se při zkoumání a sestavování vlastních šablon můžete zajímat.

* [Správa přístupu pomocí RBAC a šablon Azure Resource Manageru](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-template?azure-portal=true)
* [Používání podmínek v šablonách Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-use-conditions?azure-portal=true)
* [Integrace služby Azure Key Vault v nasazení šablony Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-use-key-vault?azure-portal=true)
* [Vytvoření propojených šablon Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-create-linked-templates?azure-portal=true)
* [Osvědčené postupy pro používání šablon Azure Resource Manageru](https://blogs.msdn.microsoft.com/mvpawardprogram/2018/05/01/azure-resource-manager?azure-portal=true)
* [Řešení běžných problémů s nasazením v Azure při použití Azure Resource Manageru](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-common-deployment-errors?azure-portal=true)

