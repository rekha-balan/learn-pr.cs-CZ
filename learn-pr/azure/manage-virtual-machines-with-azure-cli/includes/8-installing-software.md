To poslední, co chceme na našem virtuálním serveru zkusit, je instalace webového serveru. Jedním z nejjednodušších balíčků k instalaci je `nginx`.

## <a name="install-nginx-web-server"></a>Instalace webového serveru NGINX

1. Vyhledejte veřejnou IP adresu svého virtuálního počítače s Linuxem. K jejímu vyhledání můžete použít příkaz `vm list-ip-addresses`.

1. Pak otevřete připojení `ssh` k počítači, jako jste to udělali, když jste ho testovali. Pamatujte na to, že musíte předat jméno správce ("**aldis**").

1. V zobrazeném prostředí spusťte následující příkaz k instalaci webového serveru `nginx`.

```bash
sudo apt-get -y update && sudo apt-get -y install nginx
```

1. Ukončete Secure Shell.

```bash
exit
```

## <a name="retrieve-our-default-page"></a>Načtení výchozí stránky

1. V prostředí Azure Cloud Shell použijte příkaz `curl` k načtení výchozí stránky linuxového webového serveru. Případně můžete otevřít nové okno prohlížeče a přejít na veřejnou IP adresu.

```azurecli
curl 40.83.165.85
```

To se nezdaří, protože virtuální počítač s Linuxem nezveřejňuje port 80 (`http`) přes integrovanou bránu firewall. Rozhraní Azure CLI k tomu má naštěstí příkaz: `vm open-port`. 

1. V Cloud Shellu zadejte následující příkaz, který otevře port 80:

```azurecli
az vm open-port --port 80 --resource-group <rgn>[sandbox resource group name]</rgn> --name SampleVM
```

Přidání síťového pravidla a otevření portu v bráně firewall bude chvilku trvat. Znovu zkuste příkaz `curl`. Tentokrát by měl vrátit data. Můžete také zobrazit stránku v prohlížeči.

```output
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx on Debian!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx on Debian!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working on Debian. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a></p>

<p>
      Please use the <tt>reportbug</tt> tool to report bugs in the
      nginx package with Debian. However, check <a
      href="http://bugs.debian.org/cgi-bin/pkgreport.cgi?ordering=normal;archive=0;src=nginx;repeatmerged=0">existing
      bug reports</a> before reporting a new bug.
</p>

<p><em>Thank you for using debian and nginx.</em></p>


</body>
</html>
```
