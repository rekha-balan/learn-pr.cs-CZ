Vytvořili jste v Azure virtuální počítač s Linuxem. Teď ho musíte nakonfigurovat pro úlohy, které chcete přesunout do Azure.

Pokud jste mezi Azure a místní sítí nenastavili síť VPN typu site-to-site, nebudou virtuální počítače Azure z místní sítě dostupné. Pokud s Azure teprve začínáte, pravděpodobně funkční síť VPN typu site-to-site ještě nemáte. Jak se můžete připojit k virtuálnímu počítači?

## <a name="azure-vm-ip-addresses"></a>IP adresy virtuálního počítače Azure

Před chvilkou jsme viděli, že virtuální počítače Azure komunikují ve virtuální síti. Ale můžeme jim také přiřadit veřejnou IP adresu. Díky veřejné IP adrese můžeme s virtuálním počítačem komunikovat po internetu. Nebo můžeme nastavit virtuální privátní síť (VPN), která propojí naši místní síť s Azure, abyste se mohli k virtuálnímu počítači bezpečně připojit i bez zpřístupnění veřejné IP adresy. Tento přístup je popsaný v jiném modulu, a pokud vás bude zajímat, je tam i plně zdokumentovaný.

Ve výchozím nastavení se veřejné IP adresy v Azure přidělují dynamicky. To znamená, že se IP adresa může průběžně měnit. U virtuálních počítačů se IP adresa přiřazuje při jejich restartu. Pokud se chcete připojovat přímo k IP adrese a potřebujete mít jistotu, že se nezmění, můžete si připlatit za přidělení statické adresy.

Zapamatujte si tato omezení i popisovaná alternativní řešení. V tomto modulu použijete veřejnou IP adresu virtuálního počítače.

## <a name="connecting-to-the-vm-with-ssh"></a>Připojení k virtuálnímu počítači pomocí SSH

Pokud se chcete k virtuálnímu počítači připojit přes SSH, potřebujete:

- veřejnou IP adresu virtuálního počítače,
- uživatelské jméno místního účtu na virtuálním počítači,
- veřejný klíč nakonfigurovaný v tomto účtu,
- přístup k odpovídajícímu privátnímu klíči,
- port 22 otevřený na virtuálním počítači.

Vygenerovali jste pár klíčů SSH. Veřejný klíč jste přidali do konfigurace virtuálního počítače a ověřili jste, že port 22 je otevřený.

V další lekci tyto údaje použijete k otevření bezpečného terminálu na virtuálním počítači pomocí SSH.

Jakmile je terminál otevřený, máte přístup ke všem standardním linuxovým nástrojům příkazového řádku.

V dalším kroku se připojíme k virtuálnímu počítači přes SSH.