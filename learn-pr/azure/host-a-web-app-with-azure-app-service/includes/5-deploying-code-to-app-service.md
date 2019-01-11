Pojďme se podívat, jak nasadit aplikaci do služby App Service.

## <a name="what-is-automated-deployment"></a>Co je automatizované nasazení?

Automatizované nasazení neboli kontinuální integrace je proces používaný k vydávání nových funkcí a oprav chyb rychlým a opakujícím se způsobem s minimálním dopadem na koncové uživatele.

Azure podporuje přímé automatizované nasazení z různých zdrojů. Dostupné jsou tyto možnosti:

- **Visual Studio Team Services (VSTS)**: Kód můžete odeslat do služby VSTS, sestavit ho v cloudu, otestovat ho, vygenerovat z něj verzi a nakonec ho odeslat do webové aplikace Azure.

- **GitHub**: Azure podporuje automatizované nasazení přímo z GitHubu. Když připojíte úložiště GitHub k Azure kvůli automatickému nasazení, budou se všechny změny odeslané do produkční větve GitHubu nasazovat automaticky.

- **Bitbucket**: Podobně jako u GitHubu můžete automatizované nasazení nakonfigurovat i pro Bitbucket.

- **OneDrive**: Účet OneDrive můžete propojit se službou App Service, aby při změně kteréhokoli souboru v účtu OneDrive služba Azure tuto změnu automaticky rozpoznala a provedla všechny změny v souborech webové aplikace. Tato možnost je ideální pro statické weby. Budete mít dojem, že pracujete s místním systémem souborů, protože všechny změny se rychle a bez problémů promítnou do Azure.

- **Dropbox**: Podobně jako u OneDrivu můžete soubory webové aplikace hostovat v účtu Dropbox, odkud se budou automaticky odesílat a nasazovat do webové aplikace.

- **Externí úložiště**: Automatizované nasazení můžete nakonfigurovat pro jakékoli externí úložiště Git.

### <a name="non-automated-deployment-to-azure"></a>Neautomatizované nasazení do Azure

Existuje několik možností, jak odeslat kód do Azure ručně:

- **FTP/S**: Tradiční způsob odesílání kódu do libovolného hostitelského prostředí představují protokoly FTP a FTPS. Přestože se už nedoporučují, můžete je stále používat.

- **Nasazení webu / integrované vývojové prostředí (IDE)**: K publikování webové aplikace do Azure můžete použít Visual Studio IDE. Mechanismus publikování, který je součástí sady Visual Studio, používá k odeslání kódu do Azure technologii nasazení webu.

- **Git**: Azure spravuje pro vaši aplikaci **místní úložiště Git**. Svůj kód můžete **zapsat** přímo do něj.

## <a name="summary"></a>Shrnutí

Azure nabízí více způsobů nahrání kódu, abyste si mohli vybrat ten, který bude pro váš aktuální pracovní postup nejvhodnější.