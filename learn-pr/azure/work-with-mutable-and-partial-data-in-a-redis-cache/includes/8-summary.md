V tomto modulu jsme viděli, jak Azure Cache for Redis může pomoct mnoha způsoby vylepšit výkon aplikace. Nejdříve jsme zjistili, jak se dá pomocí transakcí zajistit současné vyvolání více operací bez přerušení. Pak jsme si ukázali, jak pomocí vypršení platnosti dat usnadnit vymazání dat, která už nepotřebujeme. Přesto i při použití mezipaměti můžete někdy narazit na problémy s pamětí. Předvedli jsme si, jak pomocí zásad vyřazení uvolnit místo pro nová data. Nakonec jsme se podívali na model doplňování do mezipaměti, který zajistí ukládání nejběžnějších dat do mezipaměti, aby se zkrátil čas potřebný pro jejich načtení.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]