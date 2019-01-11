Azure Active Directory (Azure AD) není jen řadič domény v cloudu. Azure AD je cloudová adresářová služba a služba pro správu identit pro více tenantů. Kombinuje základní adresářové služby, správu přístupu k aplikacím a ochranu identit do jednoho řešení. Azure AD dokáže pracovat v součinnosti s existujícím prostředím služby Windows Server Active Directory pomocí Azure AD Connect, což vám umožňuje využít investice do stávajícího místního řešení identit.

Azure AD se dodává ve čtyřech edicích. V tomto cvičení se zaměříme jen na funkce edic Azure AD Premium P1 a P2.

V tomto modulu vytvoříme adresář služby Azure AD, kde budou všichni uživatelé a skupiny podobně jako v místním adresáři.

Když vytvoříme adresář, stane se náš účet automaticky globálním správcem. Účty s právy globálního správce by měly být regulovány, protože mají úplný přístup ke všem funkcím pro správu služby Azure AD. Globálních správců může být víc, jenom jeden z nich ale může přiřazovat další role správců uživatelům.

## <a name="how-can-azure-ad-help-you-protect-access-to-applications"></a>Jak Azure AD pomáhá chránit přístup k aplikacím?

Mezi součásti služby Azure AD Premium patří vícefaktorové ověřování a zásady podmíněného přístupu. Při společném použití poskytují tyto funkce jemnější odstupňování ochrany přístupu k aplikacím.

Zásady podmíněného přístupu se skládají z těchto prvků:

- Uživatelé nebo skupiny
- Aplikace, ke kterým se snaží získat přístup
- Kontrolní mechanismy, které mají být splněny, například vícefaktorové ověřování

V této lekci jste se naučili základy služby Azure AD a zjistili jste, jaké funkce jsou dostupné pro zabezpečení přístupu k aplikacím.