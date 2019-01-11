Často je potřeba v nastavených intervalech spouštět určitou logiku. Představte si, že jste vlastník blogu a všimnete si, že vaši odběratelé nečtou vaše nejnovější příspěvky. Rozhodnete se, že nejlepší bude posílat jim jednou týdně e-mail s připomenutím novinek na vašem blogu. Tuto logiku implementujete pomocí aplikace funkcí Azure s _triggerem časovače_, který ji spustí každý týden.

## <a name="what-is-a-timer-trigger"></a>Co je trigger časovače?

Trigger časovače je aktivační událost, která spouští požadovanou funkci v pravidelných intervalech. Při vytvoření triggeru časovače musíte zadat dva základní údaje.

1. *Název parametru časového razítka*, což je jednoduše identifikátor pro použití triggeru v kódu.
2. *Plán*, což je výraz CRON, který specifikuje interval časovače.

## <a name="what-is-a-cron-expression"></a>Co je výraz CRON?

*Výraz CRON* je řetězec, který se skládá ze šesti polí, která představují sadu časů.

Pořadí těchto šesti polí v Azure je následující: `{second} {minute} {hour} {day} {month} {day of the week}`.

Například výraz CRON pro vytvoření triggeru, který se spustí každých 5 minut, bude vypadat takto:

```log
0 */5 * * * *
```

Zpočátku může být tento řetězec matoucí. Vrátíme se o kousek zpátky a podíváme se na výrazy CRON podrobněji.

K sestavení výrazu CRON potřebujete rozumět některým speciálním znakům.

| Speciální znak | Význam | Příklad |
| ------------- | ------------- | ------------- |
| *      | Vybere každou hodnotu v poli. | Hvězdička „*“ v poli pro den v týdnu znamená *každý* den. |
| ,      | Odděluje položky seznamu. | Čárka ve výrazu „1,3“ v poli pro den v týdnu znamená jen pondělky (1. dny) a středy (3. dny). |
| -      | Určuje rozsah. | Pomlčka ve výrazu „10-12“ v poli hodin znamená rozsah, který zahrnuje 10, 11 a 12 hodin. |
| /      | Určuje přírůstek. | Lomítko „*/10“ v poli minut znamená přírůstek každých 10 minut. |

Teď se vrátíme k původnímu příkladu výrazu CRON. Zkusíme si ho rozebrat po jednotlivých polích.

```log
0 */5 * * * *
```

**První pole** reprezentuje sekundy. Toto pole může nabývat hodnot 0 až 59. Protože obsahuje nulu, vybere první možnou hodnotu, což je jedna sekunda.

**Druhé pole** představuje minuty. Hodnota „*/5“ obsahuje dva speciální znaky. První, hvězdička (\*), znamená „vybrat každou hodnotu v poli“. Vzhledem k tomu, že toto pole představuje počet minut, budou možné hodnoty 0 až 59. Druhý speciální znak je lomítko (/), které představuje přírůstek. Kombinace těchto znaků znamená, že se ze všech možných hodnot 0 až 59 vybere každá pátá hodnota. Jednodušeji by se to dalo vyjádřit jako „každých pět minut“.

**Zbývající čtyři pole** představuje hodinu, den, měsíc a den v týdnu. Hvězdička v těchto polích znamená, že se vybere každá možná hodnota. V tomto příkladu vybíráme „každou hodinu každého dne v každém měsíci."

Když to celé dáme zase dohromady, znamená to „první sekundu každé páté minuty každé hodiny každého dne v každém měsíci“.

## <a name="how-to-create-a-timer-trigger"></a>Jak vytvořit trigger časovače

Na portálu Azure Portal můžete vytvořit trigger časovače. V aplikaci funkcí Azure vyberte ze seznamu šablon triggerů položku **trigger časovače**. Zadejte logiku, kterou chcete spustit. Přidejte **název parametru časového razítka** a **výraz CRON**.

V tomto modulu se zaměříme na vytváření triggerů na portálu, ale triggery můžete také vytvářet prostřednictvím kódu programu pomocí nástrojů Core, sady Visual Studio nebo editoru VS Code.

Trigger časovače bude spouštět aplikaci funkcí Azure podle pravidelného plánu. Pro definování plánu triggeru časovače jsme vytvořili výraz CRON, což je řetězec, který představuje množinu časových bodů.