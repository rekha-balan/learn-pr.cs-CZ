Azure poskytuje Šifrování služby Storage (SSE) a Azure Disk Encryption (ADE) pro zabezpečení disků virtuálních počítačů Azure. Tyto technologie společně poskytují silné 256bitové šifrování jako součást obranného přístupu pro ochranu disků virtuálních počítačů Azure. Abyste mohli povolit šifrování disku, je nutné nejprve splnit požadavek v podobě nastavení šifrování pomocí služby Azure Disk Encryption. Tento proces je možné automatizovat pomocí konfiguračního skriptu požadavků pro Azure Disk Encryption. K povolení šifrování na nových virtuálních počítačích můžete použít šablonu Azure Resource Manageru. Tím se zajistí šifrování dat při nasazení, aby se eliminovala všechna ohrožení zabezpečení.

<!-- Cleanup sandbox -->
[!include[](../../../includes/azure-sandbox-cleanup.md)]

## <a name="additional-resources"></a>Další zdroje informací

- [Řešení potíží s šifrováním disků virtuálních počítačů Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-tsg)
- [Postup šifrování virtuálního počítače s Linuxem v Azure](https://docs.microsoft.com/azure/virtual-machines/linux/encrypt-disks)
- [Přehled Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview)
- [Jaké distribuce Linuxu podporuje Azure Disk Encryption? ](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-faq#bkmk_LinuxOSSupport).
- [Šablony Azure Resource Manageru v GitHubu](https://github.com/Azure/azure-quickstart-templates)