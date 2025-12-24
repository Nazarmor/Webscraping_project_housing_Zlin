# Webscraping_project_housing_Zlin

Automated web scraper for Sreality.cz extracting apartment listings (price, area, location) to analyze housing market trends in Zlín.

📊 Analýza trhu s nájemními byty (Zlín)
Tento projekt se zaměřuje na automatizovaný sběr a analýzu dat o pronájmech bytů ve Zlíně z portálu Sreality.cz. Cílem bylo získat přehled o cenových hladinách, vlivu dispozic na cenu a identifikovat zajímavé investiční či nájemní příležitosti.

Projekt je rozdělen do tří logických fází:
1.  **Získání dat (Web Scraping):** Automatické stažení inzerátů.
2.  **Zpracování dat (Data Cleaning):** Čištění a příprava dat pro analýzu.
3.  **Analýza a vizualizace:** Výpočet statistik, grafy a hledání "výhodných" nabídek.

Na základě zpracovaných dat (cca 200+ inzerátů) vyplynulo několik zajímavých trendů pro trh ve Zlíně:

Průměrná cena nájmu: Pohybuje se okolo 15 150 Kč/měsíc.
Nejčastější dispozice: Na trhu dominují malé byty 1+kk, což naznačuje vysokou poptávku po startovacím bydlení nebo bydlení pro jednotlivce.
Cena za m²: Nejvyšší cenu za metr čtvereční mají nejmenší byty (1+kk a 1+1). Pro majitele to znamená vyšší výnosnost, pro nájemníky relativně dražší bydlení v přepočtu na plochu.
Lokalita: Mezi nejdražší ulice s vyšší koncentrací bytů patří například Mostní nebo Díly III.
Cenový rozptyl: Největší rozdíly mezi minimální a maximální cenou byly zaznamenány u dispozice 3+kk, kde se nabídka pohybuje od levnějších starších bytů až po luxusní novostavby.


💡 BONUS: Hledání nejvýhodnějších 3+kk
Speciální část analýzy se věnovala detailnímu průzkumu bytů s dispozicí 3+kk. Cílem bylo najít nabídky s nejlepším poměrem cena/výkon bez ohledu na stav bytu.

Metodika:
Byty byly rozděleny do velikostních kategorií po 10 m² (např. 61–70 m², 71–80 m²). U každé kategorie byla vypočítána průměrná cena. Následně byly identifikovány konkrétní inzeráty, které jsou nabízeny pod touto průměrnou cenou.

Výsledek:

Tato metoda odhalila "skryté příležitosti" – byty, které jsou vzhledem ke své velikosti levnější než tržní standard.
Analýza ukázala, že pouhá dispozice neurčuje cenu; velikostní kategorie hraje zásadní roli.
Výstupem je seznam konkrétních "podhodnocených" bytů, které mohou představovat výhodnou nabídku pro nájemníky hledající hodně prostoru za méně peněz.
