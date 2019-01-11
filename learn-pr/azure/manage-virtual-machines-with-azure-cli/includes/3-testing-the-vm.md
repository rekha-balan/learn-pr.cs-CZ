Vytvořený virtuální počítač bude mít přiřazenu veřejnou IP adresu, která je přístupná z internetu, a privátní IP adresu používanou v rámci datacentra Azure. Obě hodnoty získáte z bloku JSON vráceného příkazem `create`:

```json
{
   ...
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.83.165.85",
   ...
}
```

## <a name="connecting-to-the-vm-with-ssh"></a>Připojení k virtuálnímu počítači pomocí SSH

Když použijeme veřejnou IP adresu, můžeme nástrojem Secure Shell (`ssh`) rychle otestovat zprovoznění virtuálního počítače s Linuxem. Nezapomeňte, že jako jméno správce jsme nastavili `aldis`, takže ho musíme zadat. Ověřte, že jste použili veřejnou IP adresu _své_ spuštěné instance.

```azurecli
ssh <public-ip-address> -l aldis
```

> [!NOTE]
> Heslo nepotřebujeme, protože jsme při vytváření virtuálního počítače vygenerovali pár klíčů SSH. Když se poprvé připojíte do prostředí virtuálního počítače, zobrazí se výzva o pravosti hostitele. 
> 
> Je to proto, že se k IP adrese místo pomocí názvu hostitele připojujeme přímo. Když odpovíte „ano“, IP adresa se uloží jako platný hostitel a připojení může pokračovat.

```output
The authenticity of host '40.83.165.85 (40.83.165.85)' can't be established.
RSA key fingerprint is SHA256:hlFnTCAzgWVFiMxHK194I2ap6+5hZoj9ex8+/hoM7rE.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '40.83.165.85' (RSA) to the list of known hosts.
```

Potom se zobrazí vzdálené prostředí, ve kterém můžete zadávat linuxové příkazy.

```output
The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
aldis@SampleVM:~$
```

Vyzkoušejte cvičně několik příkazů a až skončíte, z prostředí se odhlaste (v prostředí zadejte `logout` nebo `exit`).