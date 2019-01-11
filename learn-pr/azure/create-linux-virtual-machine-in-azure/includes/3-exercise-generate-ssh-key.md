Než v Azure vytvoříme virtuální počítač s Linuxem, je potřeba se zamyslet nad vzdáleným přístupem. Potřebujeme se přihlásit k linuxovému webovému serveru, abychom mohli konfigurovat software a provádět údržbu. Ke správě linuxových virtuálních počítačů hostovaných v Azure se standardně používá SSH.

<!-- Activate the sandbox -->
[!include[](../../../includes/azure-sandbox-activate.md)]

## <a name="what-is-ssh"></a>Co je SSH?

Secure Shell (SSH) je protokol šifrovaného připojení, se kterým se můžete při nezabezpečených připojeních bezpečně přihlásit. SSH umožňuje připojit se ze vzdáleného umístění k prostředí terminálu pomocí síťového připojení.

Připojení SSH můžeme ověřit dvěma způsoby: **uživatelské jméno/heslo** nebo **pár klíčů SSH**.

> [!TIP]
> Protokol SSH sice nabízí šifrované připojení, ale pokud se při připojení SSH používají hesla, může se virtuální počítač stát terčem útoků hrubou silou zaměřených na uhodnutí hesel. Bezpečnější a oblíbenější způsob připojení k virtuálnímu počítači s Linuxem pomocí protokolu SSH je použití páru veřejného a soukromého klíče. Tyto klíče se také označují jako klíče SSH.

S párem klíčů SSH se můžete přihlásit k linuxovým virtuálním počítačům v Azure bez hesla. Tento přístup je bezpečnější, pokud se budete přihlašovat jenom z několika počítačů. Pokud se chcete k virtuálnímu počítači s Linuxem připojovat z různých míst, může být vhodnější použít kombinaci uživatelského jména a hesla. Pár klíčů SSH se skládá ze dvou částí: veřejného klíče a privátního klíče.

* Veřejný klíč se nachází v linuxovém virtuálním počítači nebo jakékoli jiné službě, pro kterou chcete použít šifrování veřejným klíčem. Můžete ho s kýmkoliv sdílet.

* **Privátní klíč** je to, co předkládáte vašemu linuxovému virtuálnímu počítači, když vytváříte připojení SSH, abyste ověřili vaši identitu. Zacházejte s ním jako s tajnou informací a chraňte ho, jako by to bylo heslo nebo jiný soukromý údaj.

Stejný pár veřejného a privátního klíče můžete použít pro přístup k více virtuálním počítačům a službám Azure.

## <a name="create-the-ssh-key-pair"></a>Vytvoření páru klíčů SSH

K vytvoření souborů veřejného a privátního klíče SSH v Linuxu, Windows 10 a MacOS můžete využít integrovaný příkaz `ssh-keygen`.

> [!TIP]
> Windows 10 zahrnuje klienta SSH v aktualizaci **Fall Creators Update**. Starší verze Windows vyžadují k použití SSH další software, [úplné podrobnosti najdete v této dokumentaci](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows). Alternativně můžete linuxový subsystém nainstalovat pod Windows, a získat tak stejné funkce.

> [!NOTE]
> My použijeme Azure Cloud Shell, který uloží klíče vygenerované v Azure do vašeho soukromého účtu úložiště. Pokud chcete, můžete tyto příkazy také zadat přímo v místním příkazovém prostředí. Při tomto postupu musíte upravit pokyny v tomto modulu, aby odpovídaly místní relaci.

[!include[](../../../includes/azure-sandbox-activate.md)]

Tady je minimální příkaz potřebný k vygenerování páru klíčů pro virtuální počítač Azure. Vytvoří pár veřejného a privátního klíče RSA protokolu SSH 2 (SSH-2). Minimální délka je 2048, ale pro účely tohoto výukového modulu použijeme 4096.

V prostředí Cloud Shell zadejte tento příkaz:

```bash
ssh-keygen -t rsa -b 4096
```

Nástroj vyzve k zadání názvů souborů a volitelného hesla. Pro toto cvičení stačí potvrdit výchozí hodnoty. V adresáři `~/.ssh` se vytvoří dva soubory: `id_rsa` a `id_rsa.pub`. Pokud tyto soubory už existují, přepíšou se. Existují různé možnosti, jak zadat název souboru a heslo, abyste se vyhnuli zobrazení této výzvy.

### <a name="private-key-passphrase"></a>Heslo privátního klíče

Heslo můžete zadat při generování privátního klíče. Toto heslo pak musíte zadat při použití klíče. Toto heslo se používá pro přístup k souboru privátního klíče SSH, ale není to heslo k uživatelskému účtu.

Když ke klíči SSH přidáte heslo, použije se k zašifrování klíče 128bitový standard AES. Bez dešifrovacího hesla je privátní klíč nepoužitelný.

> [!IMPORTANT]
> **Důrazně** doporučujeme, abyste heslo přidali. Pokud útočník odcizí privátní klíč, který nemá heslo, může ho použít pro přihlášení k serverům, které mají odpovídající veřejný klíč. Pokud je privátní klíč chráněn heslem, nemůže ho útočník použít. Heslo tak poskytuje další vrstvu zabezpečení pro vaši infrastrukturu na Azure.

## <a name="use-the-ssh-key-pair-with-an-azure-linux-vm"></a>Použití páru klíčů SSH u virtuálního počítače s Linuxem v Azure

Jakmile vygenerujete pár klíčů, můžete ho použít pro virtuální počítač s Linuxem v Azure. Veřejný klíč můžete zadat při vytváření virtuálního počítače nebo ho můžete přidat až po jeho vytvoření.

Obsah tohoto souboru si můžete prohlédnout v prostředí Azure Cloud Shell následujícím příkazem:

```bash
cat ~/.ssh/id_rsa.pub
```

Jde o jeden řádek, který bude vypadat zhruba takto:

```output
ssh-rsa XXXXXXXXXXc2EAAAADAXABAAABAXC5Am7+fGZ+5zXBGgXS6GUvmsXCLGc7tX7/rViXk3+eShZzaXnt75gUmT1I2f75zFn2hlAIDGKWf4g12KWcZxy81TniUOTjUsVlwPymXUXxESL/UfJKfbdstBhTOdy5EG9rYWA0K43SJmwPhH28BpoLfXXXXXGX/ilsXXXXXKgRLiJ2W19MzXHp8z3Lxw7r9wx3HaVlP4XiFv9U4hGcp8RMI1MP1nNesFlOBpG4pV2bJRBTXNXeY4l6F8WZ3C4kuf8XxOo08mXaTpvZ3T1841altmNTZCcPkXuMrBjYSJbA8npoXAXNwiivyoe3X2KMXXXXXdXXXXXXXXXXCXXXXX/ azureuser@myserver
```

Tuto hodnotu si zkopírujte, abyste ji mohli použít v dalším cvičení.

### <a name="use-the-ssh-key-when-creating-a-linux-vm"></a>Použití klíče SSH při vytvoření virtuálního počítače s Linuxem

Pokud chcete klíč SSH použít k vytvoření nového virtuálního počítače s Linuxem, musíte zkopírovat obsah veřejného klíče a doplnit ho na web Azure Portal _nebo_ zadat soubor veřejného klíče příkazem Azure CLI nebo Azure PowerShellu. Tento postup použijeme k vytvoření našeho virtuálního počítače s Linuxem.

### <a name="add-the-ssh-key-to-an-existing-linux-vm"></a>Přidání klíče SSH do stávajícího virtuálního počítače s Linuxem

Pokud jste vytvořili virtuální počítač s Linuxem, můžete do něj veřejný klíč nainstalovat příkazem `ssh-copy-id`. Jakmile je tento klíč ověřený pro SSH, umožní přístup k serveru bez hesla, i když se bude nadále zobrazovat výzva k zadání hesla pro klíč, pokud jste je nastavili.

Například pokud bychom měli virtuální počítač s Linuxem s názvem *myserver* a uživatelem *azureuser*, mohli bychom nainstalovat soubor veřejného klíče a autorizovat daného uživatele s tímto klíčem pomocí následujícího příkazu:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub azureuser@myserver
```

Když máme veřejný klíč, přejdeme na Azure Portal a vytvoříme virtuální počítač s Linuxem.
