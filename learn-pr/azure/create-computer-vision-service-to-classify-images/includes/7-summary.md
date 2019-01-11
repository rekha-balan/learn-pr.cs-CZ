
Rozhraní API pro počítačové zpracování obrazu je výkonný nástroj ke zpracování obrázků. Když použijete tuto službu místo sestavování vlastní, můžete potenciálně ušetřit významné náklady na vývoj a správu. Vy se soustřeďte na psaní obchodní logiky, aby bylo rozhodování relevantní pro vaši firmu, a extrahování bohatých informací z obrázků s cílem kategorizovat a zpracovat vizuální data nechte na rozhraní API pro počítačové zpracování obrazu.

V tomto modulu jsme analyzovali obrázky, extrahovali text a vygenerovali miniatury. K volání rozhraní API pro počítačové zpracování obrazu jsme v předplatném Azure vytvořili účet služby Cognitive Services. Tento účet nám poskytl přístupové klíče potřebné k provedení volání.

Dalším krokem bude provést volání s vlastními obrázky. Potom u každého volání upravíte parametry, abyste zjistili, jaký účinek mají na výsledky. Můžete zkusit i něco dalšího a vyzkoušet jiné operace tohoto rozhraní API, jako je například `describe` nebo `tag`.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="further-reading"></a>Další materiály

- [Přehled Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)
- [Přehled příkazů Azure CLI](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest)
- [Referenční dokumentace k rozhraní API pro počítačové zpracování obrazu](https://westus2.dev.cognitive.microsoft.com/docs/services/56f91f2d778daf23d8ec6739/operations/56f91f2e778daf14a499e1fb/console)