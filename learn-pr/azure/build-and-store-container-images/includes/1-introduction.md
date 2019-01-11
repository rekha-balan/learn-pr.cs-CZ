Azure Container Registry je spravovaná služba registrů Dockeru založená na opensourcovém nástroji Docker Registry 2.0. Container Registry je privátní, hostuje se v Azure a umožňuje sestavovat, ukládat a spravovat image pro všechny typy kontejnerových nasazení.

S Container Registry se image kontejnerů dají ukládat a načítat pomocí Docker CLI nebo Azure CLI. Integrace s portálem Azure Portal umožňuje vizuálně prohlížet image kontejnerů v registru kontejneru. V distribuovaných prostředích je možné použít funkci geografické replikace Container Registry, která distribuuje image kontejnerů do několika datacenter Azure, a zajišťuje tak lokalizovanou distribuci.

Kromě ukládání imagí kontejnerů můžete využít i úlohy Azure Container Registry, které umí sestavovat image kontejnerů v Azure. Úlohy používají standardní soubor Dockerfile, pomocí kterého se vytváří a ukládají image kontejneru v Azure Container Registry, aniž by se musely používat místní nástroje Dockeru. Díky úlohám Azure Container Registry můžete sestavovat na vyžádání nebo plně automatizovat sestavování imagí kontejnerů pomocí procesů a nástrojů DevOps.

## <a name="learning-objectives"></a>Cíle výuky

V tomto modulu se naučíte:

- Nasadit registr kontejneru Azure
- Sestavit image kontejneru pomocí úloh Azure Container Registry
- Nasadit tento kontejner do instance kontejneru Azure
- Replikovat image kontejneru do několika datacenter Azure

## <a name="prerequisites"></a>Požadavky  

- Žádné