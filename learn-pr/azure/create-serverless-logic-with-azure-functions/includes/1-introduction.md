Představte si, že pracujete pro výrobce eskalátorů, který investoval do technologie IoT, aby mohl sledovat své produkty po montáži v terénu. Dohlížíte na zpracování dat z čidel teploty umístěných v hnacích ústrojích eskalátorů. Monitorujete údaje o teplotě a pokud se hnací ústrojí příliš zahřívají, přidáte k nim příznak. V podřízených systémech tato data pomáhají určit, kdy je nutná údržba.

Vaše společnost přijímá data ze senzorů z několika umístění a z různých modelů eskalátorů. Data přicházejí různými způsoby, k nimž patří nahrání dávkových souborů, plánovaná vyžádání databáze, zprávy ve frontě a příchozí data z centra událostí. Chcete vyvinout opakovaně použitelnou službu, která dokáže zpracovat teplotní data ze všech těchto zdrojů.

Při návrhu takové služby s využitím tradičních strategií podnikových architektur byste museli předem zvážit serverovou infrastrukturu a údržbu: určit rozsah potřebného hardwaru, naplánovat jeho instalaci, domluvit se s IT na jeho správě atd. Alternativou ke vší té práci je **bezserverová architektura**. V případě bezserverové architektury spravuje zřizování a údržbu infrastruktury poskytovatel cloudových služeb a vy se můžete plně soustředit na tvorbu logiky aplikace. Klíčovou součástí nabídky bezserverové architektury od Azure je služba Azure Functions, která umožňuje spouštět v cloudu kousky kódu (neboli *funkce*) napsané ve vámi zvoleném programovacím jazyce.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Určit, jestli je architektura bez serveru vhodná pro vaše potřeby
- Vytvořit na webu Azure Portal aplikaci funkcí Azure
- Spustit funkci pomocí triggerů
- Monitorovat a testovat funkci Azure na webu Azure Portal
