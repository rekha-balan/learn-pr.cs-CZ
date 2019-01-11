V této lekci přidáte do projektu Artworks slavné obrazy od Picassa, Pollocka a Rembrandta. Obrazy označíte značkami, aby se služba Custom Vision Service naučila jednotlivé umělce od sebe rozlišovat.

1. Ve vytvořeném projektu **Artworks** vyberte na bočním panelu znaménko plus (**+**), které je vpravo od nabídky **Značky**.

     ![Snímek obrazovky s levým panelem projektu Artworks se zvýrazněným znaménkem plus](../media/2-add-tags.png)

1. Zobrazí se dialogové okno pro **pojmenování značky**. Do názvu značky zadejte *obraz* a vyberte **Uložit**. Touto operací vytvoříte v seznamu značek značku *obraz*. Pojďme přidat další značky. 

1. Opakováním prvního a druhého kroku přidejte další značky s hodnotami *Picasso*, *Pollock* a *Rembrandt*. Hotový seznam značek by měl vypadat jako na následujícím obrázku.

    ![Snímek části značek se seznamem všech značek vytvořených v předchozích krocích](../media/2-tag-list.png)

    Vidíte, že počet obrazů v projektu označených těmito značkami je nula. Teď do projektu přidáme několik obrazů a přiřadíme jim značky.

1. Stáhněte si [cvs resources.zip](https://github.com/MicrosoftDocs/mslearn-classify-images-with-the-custom-vision-service/raw/master/cvs-resources.zip) obsahující prostředky image pro tento modul a rozbalte ho do místního počítače. 

1. Zpět na portálu vyberte **Add images** (Přidat obrázky) a přidejte obrazy do projektu.

    ![Snímek obrazovky s projektem Artworks se zvýrazněným tlačítkem Add images (Přidat obrázky)](../media/2-portal-click-add-images.png)

1. Ve složce **cvs-resources**, kterou jste si stáhli místně v kroku 4, přejděte do složky „Artists\Picasso“.

1. Vyberte všechny soubory v „Artists\Picasso“ a pak vyberte možnost **Otevřít**.

    ![Snímek obrazovky se složkou Picasso a vybranými všemi soubory obrázků a zvýrazněným tlačítkem Otevřít ](../media/2-fe-browse-picasso-01.png)

1. Otevře se dialogové okno **Image upload** (Nahrání obrázku), ve kterém se zobrazí miniatury všech nahrávaných obrázků. Vyberte pole **My Tags** (Moje značky), které otevře rozevírací seznam značek, které můžete obrázkům přiřadit.

    ![Snímek obrazovky dialogového okna Image upload (Nahrání obrázku) zobrazující miniatury obrazů Picassa a rozevíracího seznamu My tags (Moje značky) se všemi značkami](../media/2-upload-picasso-tags.png)

1. Vyberte značky *obraz* a *Picasso*, pak vyberte **Upload 7 files** (Nahrát 7 souborů) a dokončete nahrávání. 

1. Ověřte, že nahrané obrázky jsou v projektu Artworks a že seznam značek byl aktualizován a je v něm vidět, že jsme značkami *Picasso* a *obraz* označili sedm obrázků.

    ![Snímek obrazovky s projektem Artworks zobrazující nahrané obrázky Značky Picasso a obraz jsou vybrané v části se značkami na levém panelu](../media/2-portal-tagged-01.png)

1. Ze sedmi Picassových obrazů dokáže služba Custom Vision Service obstojně identifikovat Picassovy obrazy. Pokud byste model začali trénovat už teď, poznal by jenom, jak vypadají Picassovy obrazy, ale nedokázal by poznat obrazy jiných umělců. Dalším krokem bude nahrání několika obrazů od jiného umělce. 

1. Vyberte **Add images** (Přidat obrázky) a vyberte v prostředcích k tomuto modulu všechny obrázky ve složce Artists\Rembrandt. Označte je značkami „obraz“ a „Rembrandt“ (nikoli „Picasso“) a nahrajte je do projektu volbou **Upload 6 files** (Nahrát 6 souborů).

    ![Snímek obrazovky dialogového okna Image upload (Nahrání obrázku) zobrazující miniatury obrazů Rembrandta a zvolené značky obraz a Rembrandt](../media/2-upload-rembrandt.png)

1. Ověřte, že se v projektu vedle Picassových obrazů zobrazují i Rembrandtovy obrazy a že v seznamu značek přibyl „Rembrandt“.

    ![Snímek obrazovky s projektem Artworks zobrazující nahrané obrázky. V části se značkami na levém panelu jsou zvolené značky Picasso, Rembrandt a obraz.](../media/2-portal-tagged-02.png)

1. Teď přidáte obrazy tajemného malíře Jacksona Pollocka, aby služba Custom Vision Service uměla rozpoznávat také jeho obrazy. Vyberte v prostředcích k tomuto modulu všechny obrázky ve složce Artists\Pollock, označte je značkami „obraz“ a „Pollock“ a nahrajte je do projektu.

Po nahrání takto označených obrázků bude dalším krokem trénování modelu pomocí těchto obrázků tak, aby uměl rozlišit obrazy Picassa, Rembrandta a Pollocka a také určit, jestli je obraz prací jednoho z těchto slavných malířů.