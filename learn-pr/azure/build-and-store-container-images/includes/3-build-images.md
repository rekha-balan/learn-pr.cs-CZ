Řekněme, že vaše společnost ke správě svých výpočetních úloh používá image kontejneru. K vytvoření imagí kontejnerů používáte místní nástroje Dockeru.

Kontejnery teď můžete vytvářet pomocí Azure Container Registry Tasks. Služba Azure Container Registry Tasks také umožňuje integrovat procesy DevOps do automatického sestavení při potvrzení zdrojového kódu.

Teď pomocí Azure Container Registry Tasks zautomatizujeme vytvoření image kontejneru.

## <a name="create-a-container-image-with-azure-container-registry-tasks"></a>Vytvoření image kontejneru v Azure Container Registry Tasks

Standardní soubor Dockerfile obsahuje pokyny pro sestavení. Služba Azure Container Registry Tasks umožňuje opakovaně používat jakýkoli Dockerfile, který právě máte ve svém prostředí, včetně vícefázových buildů.

V našem příkladu použijeme nový Dockerfile.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

Prvním krokem je vytvoření nového souboru s názvem `Dockerfile`. Soubor můžete upravit v libovolném textovém editoru. V tomto příkladu použijeme editor Cloud Shell.

1. Zadáním následujícího příkazu do okna Cloud Shellu otevřete editor.

    ```bash
    code
    ```

1. Zkopírujte následující obsah do editoru.

    ```bash
    FROM    node:9-alpine
    ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/package.json /
    ADD     https://raw.githubusercontent.com/Azure-Samples/acr-build-helloworld-node/master/server.js /
    RUN     npm install
    EXPOSE  80
    CMD     ["node", "server.js"]
    ```

1. Změny uložte pomocí kombinace kláves <kbd>Ctrl+S</kbd> (na počítači Mac <kbd>Cmd+S</kbd>). Po zobrazení výzvy soubor pojmenujte `Dockerfile`.

    Tato konfigurace přidá do image `node:9-alpine` aplikaci Node.js. Potom nakonfiguruje kontejner tak, aby poskytoval aplikaci na portu 80 prostřednictvím příkazu *EXPOSE*.

1. Spusťte následující příkaz Azure CLI a sestavte image kontejneru ze souboru Dockerfile. *$ACR_NAME* je proměnná, kterou jste definovali v předchozí lekci pro uchovávání názvu registru kontejneru.

    ```azurecli
    az acr build --registry $ACR_NAME --image helloacrtasks:v1 .
    ```

    > [!NOTE]
    > Nezapomeňte na konec předchozího příkazu doplnit tečku (`.`). Představuje zdrojový adresář, který obsahuje dockerový soubor – v našem případě se jedná o aktuální adresář. Vzhledem k tomu, že jsme nezadali název souboru s parametrem --file, příkaz v našem aktuálním adresáři vyhledá soubor s názvem **Dockerfile**.

## <a name="verify-the-image"></a>Ověření image

1. V Cloud Shellu spusťte následující příkaz, kterým ověříte, že jste image vytvořili a uložili do registru.

    ```azurecli
    az acr repository list --name $ACR_NAME --output table
    ```

    Výstup tohoto příkazu by měl vypadat nějak takto:
    
    ```console
    Result
    -------------
    helloacrtasks
    ```

Image `helloacrtasks` je teď připravená k použití.
