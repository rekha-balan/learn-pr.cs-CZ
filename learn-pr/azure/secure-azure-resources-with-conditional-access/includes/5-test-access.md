V předchozích cvičeních jsme vytvořili adresář, uživatele a skupinu a pak jsme vytvořili pravidlo podmíněného přístupu, které při přístupu k webu Azure Portal vyžaduje Azure Multi-Factor Authentication. Teď otestujeme, jestli máme přístup k prostředkům.

## <a name="test-access-to-resources"></a>Otestování přístupu k prostředkům

Víte, že uživatelé se k aplikacím SaaS budou přihlašovat a přistupovat pomocí portálu Moje aplikace, což otestujeme.

1. Otevřete okno prohlížeče v režimu InPrivate.

1. Přejděte na adresu https://myapps.microsoft.com.

1. Přihlaste se jako uživatel, kterého jsme vytvořili v lekci 3.

   * Všimněte si, že jste k portálu přihlášení bez potřeby vícefaktorového ověřování.

1. Ve stejném okně prohlížeče přejděte na adresu https://portal.azure.com.

   * Všimněte si, že teď musíte zadat další informace, aby váš účet zůstal zabezpečený. Důvodem tohoto přerušení je aktivace služby Azure Multi-Factor Authentication, protože jsme vytvořili zásady podmíněného přístupu. V tento okamžik můžete skončit a zavřít okno prohlížeče.