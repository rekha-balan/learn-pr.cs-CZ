Nasazení kontejnerů ve službě Azure Container Instances je jednoduché a rychlé. Proto je tato platforma skvělá pro úkoly, které se spouští jen jednou, jako je vykreslení obrázku, nebo sestavení a testování aplikací.

Konfigurovatelné zásady restartování umožňují nastavit, aby se kontejnery po dokončení procesů zastavily. Instance kontejnerů se účtují po sekundách. Proto se vám účtují jenom výpočetní prostředky používané po dobu, kdy kontejner běží a provádí vaši úlohu.

## <a name="what-are-container-restart-policies"></a>Co jsou zásady restartování kontejneru?

Služba Azure Container Instances nabízí tři možnosti zásad restartování:

| Zásada restartování   | Popis |
| ---------------- | :---------- |
| **Vždy** | Kontejnery ve skupině kontejnerů se restartují vždy. Tato zásada dává smysl pro dlouho běžící úlohy, jako je webový server. Jde o **výchozí** nastavení, které se použije vždy, když při vytvoření kontejneru nezadáte zásadu restartování. |
| **Never** (Nikdy) | Kontejnery ve skupině kontejnerů se nerestartují nikdy. Kontejnery se spouštějí jenom jednou. |
| **OnFailure** (Při chybě) | Kontejnery ve skupině se restartují jen v případě, že proces spuštěný v kontejneru nebude úspěšný (když skončí nenulovým ukončovacím kódem). Kontejnery se spouštějí aspoň jednou. Tato zásada dobře funguje pro kontejnery, které provádějí krátkodobé úlohy. |

## <a name="run-a-container-to-completion"></a>Běh kontejneru do dokončení

Pokud se chcete podívat, jak zásada restartování funguje v praxi, vytvořte z image Dockeru **microsoft/aci-wordcount** instanci kontejneru a nastavte zásadu restartování **OnFailure** (Při chybě). Tento kontejner spustí skript v jazyce Python, který analyzuje text Shakespearova Hamleta, zapíše 10 nejčastěji používaných slov do standardního výstupu a pak skončí.

1. Spusťte kontejner tímto příkazem `az container create`:

    ```azurecli
    az container create \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name mycontainer-restart-demo \
      --image microsoft/aci-wordcount:latest \
      --restart-policy OnFailure \
      --location eastus
    ```

    Služba Azure Container Instances spustí kontejner a po skončení jeho procesu (v tomto případě skriptu) ho zastaví. Když služba Azure Container Instances zastaví kontejner, který má zásadu restartování nastavenou na **Never** (Nikdy) nebo **OnFailure** (Při chybě), stav kontejneru se nastaví na **Terminated** (Ukončeno).

1. Spuštěním příkazu `az container show` zkontrolujte stav kontejneru.

    ```azurecli
    az container show \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name mycontainer-restart-demo \
      --query containers[0].instanceView.currentState.state
    ```

    Příkaz opakujte, dokud se nedosáhne stavu **Terminated** (Ukončeno).

1. Zobrazte protokoly kontejneru a prozkoumejte výstup. Uděláte to spuštěním příkazu `az container logs`, jako je tento:

    ```azurecli
    az container logs \
      --resource-group <rgn>[sandbox resource group name]</rgn> \
      --name mycontainer-restart-demo
    ```

    Uvidíte toto:

    ```json
    [('the', 990),
     ('and', 702),
     ('of', 628),
     ('to', 610),
     ('I', 544),
     ('you', 495),
     ('a', 453),
     ('my', 441),
     ('in', 399),
     ('HAMLET', 386)]
    ```