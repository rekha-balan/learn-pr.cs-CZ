MEAN je vývojový zásobník určený k vytváření a hostování webových aplikací. Určitě si vzpomínáte, že MEAN je zkratka jednotlivých komponent: MongoDB, Express, AngularJS a Node.js.

V tomto modulu jste se dozvěděli, kdy je vhodné k vývoji webu použít zásobník MEAN a kdy zvolit jiné řešení. Při vašich úvahách o zásobníku MEAN může být hlavním důvodem znalost JavaScriptu.

Seznámili jste se s fungováním zásobníku MEAN tím, že jste v Azure vytvořili virtuální počítač s Ubuntu a nainstalovali do něj zásobník MEAN pro vývoj webu.

V zásobníku MEAN jste vytvořili základní webovou aplikaci, která slouží k evidenci knih. Tato webová aplikace používá:

* **MongoDB** k ukládání informací o knihách.
* **Express** k přesměrování každého požadavku HTTP na příslušnou obslužnou rutinu.
* **AngularJS** k připojení uživatelského rozhraní k obchodní logice programu.
* **Node.js** k hostování serverové aplikace.

Pokud [hledáte zdrojový kód webové aplikace](https://github.com/MicrosoftDocs/mslearn-build-a-web-app-with-mean-on-a-linux-vm?azure-portal=true), najdete ho na GitHubu.

[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="learn-more"></a>Další informace

V tomto modulu jste se dozvěděli, jak funguje zásobník MEAN, a vytvořili jste základní webovou aplikaci, která ho používá. V dalším kroku začnete vytvářet aplikace, které řeší vaše vlastní obchodní potřeby. Tyto aplikace pak můžete nasadit do Azure a pomocí automatických procesů je monitorovat a zdokonalovat. Tady jsou některé zdroje, kde najdete více informací.

### <a name="learn-more-about-mean-stack-application-development"></a>Další informace o vývoji aplikací v zásobníku MEAN

Přečtěte si další informace o komponentách zásobníku MEAN a dalších balíčcích Node.js použitých v tomto modulu.

* [MongoDB](https://www.mongodb.com?azure-portal=true)
* [Express](http://expressjs.com?azure-portal=true)
* [AngularJS](https://angularjs.org?azure-portal=true)
* [Node.js](https://nodejs.org?azure-portal=true)

* [npm](https://www.npmjs.com?azure-portal=true)
* [Mongoose](https://www.npmjs.com/package/mongoose?azure-portal=true)
* [body-parser](https://www.npmjs.com/package/body-parser?azure-portal=true)

### <a name="learn-about-the-azure-web-apps-service"></a>Další informace o službě Azure Web Apps

V tomto modulu jste k hostování webové aplikace použili virtuální počítač. Prostředí virtuálního počítače je ovladatelnější, a proto může být ideální pro vaše aktuálně spravovaná nasazení. Existují ale i jiné způsoby hostování webových aplikací. V článku [Vytvoření webové aplikace Node.js ve službě Azure](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-nodejs?azure-portal=true) se dozvíte, jak zjednodušit nasazení aplikací použitím služby Azure Web Apps.

### <a name="automate-your-deployments"></a>Automatické nasazení

V tomto modulu jste konfigurovali virtuální počítač a spouštěli aplikaci převážně ručně. Až budete mít s tímto postupem více zkušeností, může ho automatizovat. Nasazení změn bude rychlejší a spolehlivější. Podívejte se na článek [Vytvoření kanálu CI/CD pro Node.js pomocí služby Azure DevOps Project](https://docs.microsoft.com/azure/devops-project/azure-devops-project-nodejs?azure-portal=true), ve kterém se dozvíte, jak používat AzureDevOps k nasazení aplikace Node.js prostřednictvím nepřetržitě integrovaného a poskytovaného kanálu (CI/CD).