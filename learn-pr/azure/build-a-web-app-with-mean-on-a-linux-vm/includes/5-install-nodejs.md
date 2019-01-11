Tady nainstalujete modul runtime Node.js, který ve zkratce MEAN představuje písmeno **N**. Node.js je stejně jako MongoDB typu Open Source. 

Node.js bude fungovat jako server, který hostuje vaši webovou aplikaci a zpracovává příchozí provoz HTTP. Node.js také nabízí způsob komunikace s instalací databáze MongoDB, o které si povíme za chvilku.

## <a name="what-versions-of-nodejs-are-available"></a>Jaké verze Node.js jsou k dispozici?

Existují dvě možnosti, jak získat Node.js:

- **Dlouhodobě podporovaná verze (LTS)** – LTS se obecně považuje za stabilnější verzi, a proto je doporučená pro většinu uživatelů a produkčních prostředí.
- **Aktuální** – aktuální verze je určena uživatelům, kteří chtějí experimentovat s nejnovějšími funkcemi. Mezi jednotlivými verzemi můžou být zásadní změny, a proto se tato verze nedoporučuje do produkčních prostředí.

Tady použijeme verzi LTS Node.js.

## <a name="how-do-i-install-nodejs"></a>Jak nainstalovat Node.js

Stejně jako MongoDB můžete i Node.js spustit v systémech Windows, macOS a Linux. Node.js také podporuje unixové operační systémy, jako je SunOS a AIX.

Stejně jako v případě MongoDB si úložiště Node.js zaregistrujete, aby správce balíčků `apt` tento balíček našel.

Nezapomeňte, že pracujete s virtuálním počítačem s Ubuntu. Později se můžete [podívat do instalační příručky](https://nodejs.org/en/download/package-manager?azure-portal=true), ve které se dozvíte, jak Node.js nainstalovat na vaši oblíbenou platformu.

## <a name="install-nodejs"></a>Instalace Node.js

Tady nainstalujete Node.js. Stejně jako v případě MongoDB postup zahrnuje i registraci úložiště Node.js, aby správce balíčků `apt` tento balíček našel.

> [!IMPORTANT]
> Tady budete pracovat s připojením SSH k virtuálnímu počítači s Ubuntu, který jste vytvořili v předchozí části tohoto modulu.

1. Zaregistrujte úložiště Node.js, aby správce balíčků dokázal podobné balíčky najít.

    ```bash
    curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
    ```

1. Nainstalujte balíček Node.js.

    ```bash
    sudo apt-get install -y Node.js
    ```

1. Ověřte instalaci příkazem `node -v`.

    ```bash
    node -v
    ```

    Z výstupu poznáte, že máte nejnovější verzi LTS modulu runtime Node.js.

## <a name="exit-your-ssh-session"></a>Ukončení relace SSH

Všechno, co jste potřebovali udělat přímo na virtuálním počítači, je hotové. Spusťte příkaz `exit` a ukončete relaci SSH s virtuálním počítačem.

```bash
exit
```

Teď jste zpátky v relaci editoru Cloud Shell.