![Logo Dockeru](../media/3-image1.PNG)

Docker je nástroj, který vám umožňuje nasadit vaše aplikace v sandboxu, aby mohly být spuštěny na hostitelském operačním systému podle vašeho výběru. Umožňuje zabalit vaši aplikaci se všemi jejími závislosti do standardizované jednotky. Co když se ale základní image DSVM dodává s předem nainstalovanou nejoblíbenější architekturou hloubkového učení, proč byste používali Docker?

Při úlohách zahrnujících hloubkové učení vývojáři narážejí na nástrahy v podobě závislostí. Příklad: 

- Nutnost sestavovat vlastní balíčky – Výzkumníci v oblasti hloubkového učení při publikování kódu do GitHubu často nemyslí na produkční prostředí. Pokud jim balíček funguje v jejich vlastním vývojovém prostředí, často předpokládají, že budete fungovat i ostatním.
- Správa verzí ovladačů GPU – CUDA je aplikační programovací rozhraní (API) a platforma paralelních výpočtů. Za vývojem architektury CUDA stojí společnost NVIDIA. Umožňuje vývojářům používat grafický procesor (GPU) s podporou architektury CUDA pro obecné účely zpracování. Některé verze Tensorflow nebudou s verzí CUDA 9.1 a vyšší fungovat. Další architektury, jako je například PyTorch, budou pravděpodobně fungovat lépe s novějšími verzemi CUDA.

K vyřešení těchto problémů a zlepšení použitelnosti kódu můžete použít Docker nebo jeho variantu pro GPU, NVIDIA Docker, pro správu a provozování projektů hloubkového učení. 

<!--Quiz 
What is CUDA? 
What versioning issues do deep learning engineers deal with? -->