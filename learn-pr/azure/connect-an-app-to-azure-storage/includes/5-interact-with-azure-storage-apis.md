Azure Storage poskytuje REST API, které pracuje s kontejnery a daty uloženými v každém účtu. K dispozici jsou nezávislá rozhraní API, která spolupracují s každým typem dat, který můžete uložit. Připomínáme, že existují čtyři konkrétní datové typy:

- **Objekty blob** pro nestrukturovaná data jako například binární a textové soubory
- **Fronty** pro trvalé zasílání zpráv
- **Tabulky** pro strukturované úložiště klíčů nebo hodnot
- **Soubory** pro tradiční sdílené složky protokolu SMB

## <a name="using-the-rest-api"></a>Použití rozhraní REST API

Rozhraní REST API Azure Storage jsou přístupná odkudkoli z Internetu, z libovolné aplikace, která může odeslat požadavek HTTP/HTTPS a přijmout odpověď HTTP/HTTPS.

Kdybyste například chtěli vypsat všechny objekty blob v kontejneru, odeslali byste něco takového:

```http
GET https://[url-for-service-account]/?comp=list&include=metadata
```

Vrátil by se vám blok XML s daty daného účtu:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<EnumerationResults AccountName="https://[url-for-service-account]/">  
  <Containers>  
    <Container>  
      <Name>container1</Name>  
      <Url>https://[url-for-service-account]/container1</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 18:09:03 GMT</Last-Modified>  
        <Etag>0x8CAE7D0C4AF4487</Etag>  
      </Properties>  
      <Metadata>  
        <Color>orange</Color>  
        <ContainerNumber>01</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container2</Name>  
      <Url>https://[url-for-service-account]/container2</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8C24928</Etag>  
      </Properties>  
      <Metadata>  
        <Color>pink</Color>  
        <ContainerNumber>02</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
    <Container>  
      <Name>container3</Name>  
      <Url>https://[url-for-service-account]/container3</Url>  
      <Properties>  
        <Last-Modified>Sun, 24 Sep 2018 17:26:40 GMT</Last-Modified>  
        <Etag>0x8CAE7CAD8EAC0BB</Etag>  
      </Properties>  
      <Metadata>  
        <Color>brown</Color>  
        <ContainerNumber>03</ContainerNumber>  
        <SomeMetadataName>SomeMetadataValue</SomeMetadataName>  
      </Metadata>  
    </Container>  
  </Containers>  
  <NextMarker>container4</NextMarker>  
</EnumerationResults>  
```

Tento přístup však vyžaduje spoustu ručního analyzování a vytváření paketů protokolu HTTP, aby spolupracoval s každým rozhraním API. Azure proto poskytuje předem připravené _klientské knihovny_, které práci s touto službou u běžných jazyků a architektur usnadňují.

## <a name="using-a-client-library"></a>Použití klientské knihovny

Klientské knihovny mohou vývojářům aplikací ušetřit spoustu práce, protože rozhraní API je otestované a častokrát poskytuje lepší obálky datových modelů odesílaných a přijímaných rozhraním REST API.

:::row:::  
    :::column:::  
    Microsoft má klientské knihovny Azure, které podporují různé jazyky a architektury, včetně:
    - .NET
    - Java
    - Python
    - Node.js
    - Go
    :::column-end:::
    :::column:::
        <br> ![Sample logos of supported frameworks you can use with Azure](../media/4-common-tools.png)
    :::column-end:::  
:::row-end:::  

Kdybyste chtěli například získat stejný seznam objektů blob v C#, mohli byste použít následující fragment kódu:

```csharp
CloudBlobDirectory directory = ...;
foreach (IEnumerable<IListBlobItem> blob in directory.ListBlobs(
                useFlatBlobListing: true,
                blobListingDetails: BlobListingDetails.All))
{
    // Work with blob item .. could be page blob, block blob, etc.
}
```

Nebo v JavaScriptu:

```javascript
const containerName = "...";
const blobService = storage.createBlobService();

blobService.listBlobsSegmented(containerName, null, function (error, results) {
    if (results) {
        for (var i = 0, blob; blob = results.entries[i]; i++) {
            // Work with blob item .. could be page blob, block blob, etc.
        }
    }
});
```

> [!NOTE]
> Klientské knihovny jsou pouze tenké _obálky_ rozhraní REST API. Dělají to stejné, co byste dělali vy, kdybyste používali webové služby přímo. Tyto knihovny jsou navíc open source, takže jsou velmi transparentní. Vyhledejte si je na GitHubu.

Pojďme do naší aplikace přidat podporu klientských knihoven.
