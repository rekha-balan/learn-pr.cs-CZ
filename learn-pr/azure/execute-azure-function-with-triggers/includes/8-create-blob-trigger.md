V této lekci vytvoříme funkci Azure, která při vytvoření nebo aktualizaci objektu blob zobrazí jeho název a velikost.

## <a name="create-a-blob-trigger"></a>Vytvoření triggeru objektu blob

Budeme dál používat existující aplikaci Azure Functions a přidáme do ní trigger objektu blob.

1. Zkontrolujte, že jste přihlášeni k portálu [Azure Portal](https://portal.azure.com/learn.docs.microsoft.com?azure-portal=true) pomocí stejného účtu, kterým jste aktivovali izolovaný prostor (sandbox).

1. Přejděte na obrazovku **Všechny prostředky** a vyberte vaši aplikaci funkcí.

1. Přejděte na **Funkce** a vyberte ikonu se znaménkem plus (+).

1. Vyberte **Trigger objektu blob**.

1. Pokud se zobrazí zpráva **Rozšíření nejsou nainstalovaná**, vyberte **Nainstalovat**. Instalace závislostí může trvat několik minut, proto buďte trpěliví. Než budete pokračovat, počkejte, až se instalace dokončí.

1. **Název** ponechejte výchozí.

1. **Cestu** ponechejte výchozí.

1. Vedle rozevíracího seznamu _Připojení účtu úložiště_ vyberte odkaz **nové**. V zobrazeném dialogovém okně pro výběr **Účet úložiště** vyberte účet úložiště pro tuto aplikaci funkcí.

1. Jakmile se vrátíte na obrazovku Nová funkce, vytvořte výběrem možnosti **Vytvořit** novou funkci.

## <a name="create-a-blob-container"></a>Vytvoření kontejneru objektů blob

Teď, když jsme vytvořili trigger objektu blob, můžeme pomocí Průzkumníku služby Storage vytvořit objekt blob a aktivovat funkci.

1. Na nové záložce otevřete účet úložiště, který jste použili (nebo vytvořili).

    > [!TIP]
    > Ve většině prohlížečů můžete duplikovat záložku tak, že na ni kliknete pravým tlačítkem myši a v zobrazené nabídce vyberete **Duplikovat**. Chceme použít novou záložku, abychom mohli přepínat mezi dvěma službami, se kterými pracujeme.

1. Vyberte **Účty úložiště** na postranním panelu nebo na postranním panelu vyberte **Všechny prostředky** a potom filtrujte podle názvu.

1. Klikněte na oddíl **Průzkumník služby Storage (Preview)**. Tím otevřete nový panel, kde můžete pracovat s objekty blob a soubory.

Trigger objektu blob totiž monitoruje pouze umístění zadané v poli **Cesta**. Ve výchozím nastavení by naše cesta měla být:

> samples-workitems/{name}

Musíme vytvořit kontejner s názvem **samples-workitems**.

1. Klikněte pravým tlačítkem na **KONTEJNERY OBJEKTŮ BLOB** a vyberte **Vytvořit kontejner objektů blob**.

1. Jako název zadejte **samples-workitems** a úroveň přístupu nechejte na výchozím nastavení **Privátní**. Pak vyberte **OK**.

## <a name="turn-on-your-blob-trigger"></a>Zapnutí triggeru objektu blob

Teď když jsme vytvořili kontejner pro monitorování, můžeme spustit naši funkci, abychom si prohlédli výstup při vytvoření objektu blob.

1. Přepněte zpět na záložku prohlížeče s vaší funkcí Azure Functions (nebo ji znovu otevřete).

1. Výběrem triggeru objektu blob otevřete obrazovku s kódem.

1. Otevřete záložku **protokolů** v dolní části obrazovky.

## <a name="create-a-blob"></a>Vytvoření objektu blob

Trigger objektu blob nyní běží a naslouchá aktivitě. Pojďme vytvořit objekt blob, abychom zjistili, jestli získáme zprávu protokolu.

1. Přepněte zpátky na záložku prohlížeče s Průzkumníkem služby Storage.

1. V Průzkumníku služby Storage vyberte v seznamu **KONTEJNERY OBJEKTŮ BLOB** kontejner **samples-workitems**.

1. Na panelu nástrojů vyberte **Nahrát**.

1. Vyberte libovolný soubor z počítače.

1. V nabídce **Typ ověřování** vyberte **SAS**.

1. Vyberte **Nahrát**.

1. Přepněte zpět na záložku Funkce Azure Functions a vyhledejte ve výstupních protokolech zprávu o nahrání souboru. Trigger objektu blob by se měl automaticky provést. Všimněte si, že pokud kliknete na tlačítko **Spustit** v okně funkce, pravděpodobně dojde k chybě z důvodu výchozí hodnoty, která je zadaná v textu požadavku **Test**. Pro úspěšné spuštění testovacího dialogu bude nutné v okně textu požadavku změnit cestu zadáním platného souboru.