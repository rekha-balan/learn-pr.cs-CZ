Pokud chcete pochopit, co se může při správě tajných konfiguračních kódů zvrtnout, stačí se podívat na příběh hlavního vývojáře Štěpána.

Štěpán byl na této pozici ve společnosti dodávající krmení pro domácí miláčky už několik týdnů. Zkoumal podrobnosti webové aplikace této společnosti – webové aplikace v .NET Core, která ukládá informace o objednávkách do databáze Azure SQL a k účtování pomocí platebních karet a k mapování adres zákazníků používá rozhraní API třetích stran – když nepředvídaně vložil připojovací řetězec objednávek do veřejného fóra.

O pár dní později účetní systém zaregistroval, že společnost dodala spoustu krmení, za které nikdo nezaplatil. Někdo využil připojovací řetězec k přístupu do databáze, zpětně analyzoval schéma databáze a zadal objednávky, aniž by šel přes web.

Když si Štěpán svoji chybu uvědomil, rychle změnil heslo databáze, aby narušitele webu odblokoval. Přihlásil se přímo k aplikačnímu serveru a změnil konfiguraci aplikace (místo aby ji znovu nasazoval), ale server dál vykazoval závadné žádosti.

Štěpán zapomněl, že na různých serverech běží více instancí aplikace, a změnil konfiguraci jenom pro jednu z nich. Bylo nutné provést úplné opětovné nasazení, což způsobilo další 30minutový výpadek.

Únik databázového připojovacího řetězce, klíče API nebo hesla služby může mít katastrofální následky. Potenciálními důsledky můžou být odcizená nebo odstraněná data, finanční ztráta, výpadek aplikace a nenapravitelné poškození obchodních aktiv a reputace. Tajné hodnoty se bohužel musí často nasadit současně na více míst a změnit je v nevhodnou dobu. A *někam* je musíte uložit! Podívejme se, jak se to dá všechno udělat bezpečně – s Azure KeyVault.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Prozkoumat, jaké typy informací je možné ukládat ve službě Azure Key Vault
- Vytvořit službu Azure Key Vault a použít ji k uložení hodnot konfigurace tajných kódů
- Povolit zabezpečený přístup k trezoru z webové aplikace Azure App Service se spravovanými identitami pro prostředky Azure
- Implementovat webovou aplikaci, která z trezoru načte tajné kódy
