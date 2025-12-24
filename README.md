# Webscraping_project_housing_Zlin

Automated web scraper for Sreality.cz extracting apartment listings (price, area, location) to analyze housing market trends in Zlín.

📊 Analýza trhu s nájemními byty (Zlín)
Tento projekt se zaměřuje na automatizovaný sběr a analýzu dat o pronájmech bytů ve Zlíně z portálu Sreality.cz. Cílem bylo získat přehled o cenových hladinách, vlivu dispozic na cenu a identifikovat zajímavé investiční či nájemní příležitosti.

## ⚙️ Metodika

Kód je rozdělen do tří logických kroků: **Sběr**, **ETL (Transformace)** a **Analýza**.

### 1. Web Scraping (Sběr dat)
Pro získání dat byl vyvinut vlastní scraper v Pythonu (`BeautifulSoup`, `Requests`).
* **Robustnost:** Skript ošetřuje paginaci (prochází všechny stránky výpisu).
* **Etika:** Implementován `time.sleep`, aby nedocházelo k přetěžování serveru.
* **Získaná data:** Cena, dispozice, rozměry ($m^2$), lokalita.

### 2. Klíčové kroky úpravy dat (ETL):
Surová data byla zpracována pomocí knihovny **Pandas**. Klíčové kroky čištění:
* **Odstranění nekompletních dat:** Vyřazení inzerátů, které neměly uvedenou cenu nebo jasnou dispozici (např. nabídky pouze na pronájem lůžka či pokoje).
* **Doplnění lokality:** Automatické doplnění města "Zlín" u záznamů, kde tento údaj chyběl.
* **Rozdělení adresy:** Rozčlenění původního textu lokality na dva samostatné sloupce (`Ulice` a `Město`) pro detailnější filtrování.
* **Převod formátů:** Změna textových údajů (ceny a rozměry) na číselný formát (int), což umožnilo matematické operace.
* **Nová metrika:** Výpočet sloupce `Cena za m²`, který slouží k objektivnímu srovnání cenové hladiny různě velkých bytů.

### 3. Analýza a vizualizace dat
Po vyčištění dat následovala fáze hledání souvislostí a trendů, kde byla čistá data převedena na užitečné informace.
Postup při analýze:
* **Identifikace cenových trendů:** Zkoumání, jak se mění cena nájmu v závislosti na velikosti bytu. Zde byl odhalen vztah mezi celkovou cenou a cenou za metr čtvereční.
* **Segmentace dat:** Pro férové porovnání (zejména u bytů 3+kk) byla použita metoda rozdělení do kategorií po 10 $m^2$, aby se porovnávaly jen srovnatelné nemovitosti.
* **Grafické znázornění:** Využití knihoven `Seaborn` a `Matplotlib` pro vizualizaci rozložení cen a odhalení odlehlých hodnot (tzv. outliers).
---
## 📊 Analytická zjištění a trendy

Analýza 209 inzerátů odhalila několik klíčových tržních mechanismů, které ovlivňují cenu nájmů ve Zlíně.

### 1. Paradox ceny za m² (Efekt velikosti)
Data jasně ukázala nepřímou úměru mezi velikostí bytu a cenou za metr čtvereční.
* **Zjištění:** Byty 1+kk jsou v absolutních číslech nejlevnější (průměr cca 11 800 Kč), ale v přepočtu na $m^2$ jsou suverénně nejdražší (~358 Kč/m²).
* **Analytické zdůvodnění:** Tento jev lze vysvětlit tím, že každý byt (bez ohledu na velikost) musí obsahovat drahé fixní prvky (koupelna, kuchyňská linka, toaleta). U malých bytů se cena těchto prvků rozpočítává do menší plochy, což drasticky zvedá jednotkovou cenu. Velké byty naopak nabízejí "množstevní slevu" na obytný prostor.

### 2. Struktura trhu (Dominance malých bytů)
Analýza četnosti jednotlivých dispozic ukázala, že trh ve Zlíně je orientován primárně na menší bydlení.
* **Zjištění:** Nejčastěji nabízeným typem bytu je **1+kk** (zastoupeno 51 inzeráty), následováno kategoriemi 2+kk a 2+1.

**Interpretace:**
* **Ekonomika poptávky (Startovací bydlení):** Malé byty představují finančně nejdostupnější variantu. Jsou ideální nejen pro jednotlivce (singles), ale i pro mladé páry, pro které je sdílení nákladů v malém bytě nejlevnějším způsobem bydlení.
* **Strategie investorů (Strana nabídky):** Vysoký počet těchto bytů koreluje se zjištěním, že malé byty mají nejvyšší výnos z metru čtverečního. Pro majitele nemovitostí je tedy ekonomicky nejracionálnější rekonstruovat objekty na více malých jednotek (1+kk) než na méně velkých, čímž maximalizují svůj zisk.

### 3. Geografické vlivy (Lokalita)
Lokalita se ukázala jako silný faktor cenotvorby. Z analýzy vyplynulo několik prémiových ulic.
* **Nejdražší ulice:** Mezi cenově nejvýše postavené patří **Mostní** a **Díly III** (průměr nad 20 000 Kč).

### 4. Cenová anomálie u kategorie 3+kk a hledání "Hidden Gems"

Při hloubkové analýze byl u kategorie **3+kk zaznamenán nejvyšší cenový rozptyl.** Vzhledem k již prokázanému vlivu velikosti na cenu (viz bod 1) bylo nutné ověřit, jak moc se liší výměry jednotlivých bytů v této skupině.

**Příčina cenových rozdílů (Analýza rozlohy):**
Data potvrdila, že kategorie 3+kk je rozměrově velmi nekonzistentní.
* **Nejmenší nalezený byt:** 58 m²
* **Největší nalezený byt:** 143 m²

Mezi těmito extrémy je propastný rozdíl, což přímo deformuje průměrnou cenu. Prosté srovnání "průměrného 3+kk" by proto bylo zavádějící (srovnávaly by se byty s dvojnásobnou výměrou).

**Řešení (Algoritmus pro segmentaci):**
Pro objektivní posouzení výhodnosti nabídek byl vytvořen algoritmus, který:
1.  **Segmentuje:** Dělí byty do kategorií po **10 m²**, aby se porovnávaly jen srovnatelné nemovitosti.
2.  **Čistí:** Ignoruje kategorie s nedostatečným vzorkem (např. pouze 1 inzerát).
3.  **Filtruje:** Identifikuje byty, které jsou levnější než průměr jejich specifické velikostní kategorie.

**🎯 Konkrétní výsledky analýzy:**
Algoritmus identifikoval zajímavé tržní anomálie:
* **Úspěšnost:** Z celkových 26 inzerátů typu 3+kk bylo **9 označeno jako "podhodnocených"** (pod průměrem své kategorie).
* **Nejvyšší úspora:** Rekordmanem se stal byt o výměře **100 m²**, jehož cena byla o cca **7 280 Kč nižší**, než je standard pro tuto dispozici.
* **Nejlepší kategorie:** Nejvíce příležitostí (celkem 3 byty) se nacházelo v kategorii **91–100 m²**.

---

### ⚠️ Interpretace dat a limity modelu
Je důležité zdůraznit, že tento model je **čistě kvantitativní** a porovnává pouze dvě proměnné: **Cenu** a **Užitnou plochu**.

Algoritmus označuje byty, které jsou *matematicky* výhodné, ale nezohledňuje kvalitativní faktory, které strojově čitelná data často neobsahují:
* **Stav nemovitosti:** Levnější byt může být před rekonstrukcí, zatímco dražší byt může být novostavba.
* **Lokalita v rámci města:** Model nerozlišuje mezi prémiovou čtvrtí a okrajem sídliště.
* **Vybavení a patro:** Balkon, výtah, sklep či kompletní vybavení nábytkem.

**Závěr:** Seznam "Hidden Gems" neslouží jako okamžité doporučení "berte všemi deseti", ale jako **efektivní filtr pro scouting**. Umožňuje uživateli zaměřit pozornost na byty, které mají podezřele dobrou cenu, a následně manuálně ověřit, zda jde o skutečnou příležitost, nebo zda nízká cena odráží horší stav nemovitosti.

---

## 🔮 Plány do budoucna a vylepšení

Aktuální verze projektu slouží jako analytická studie. Dalším krokem je transformace skriptu do podoby **interaktivního nástroje** pro běžné uživatele.

**Navrhované funkce:**

### 1. Generalizace algoritmu "Hidden Gems"
Rozšíření logiky, která byla použita u bytů 3+kk, na **všechny typy dispozic** (1+kk, 2+1, atd.).
* **Princip:** Skript by automaticky vytvořil velikostní kategorie (bins) pro každou dispozici zvlášť.
* **Cíl:** Okamžitě identifikovat podhodnocené nabídky napříč celým trhem, nehledě na velikost bytu.

### 2. Interaktivní konzolová aplikace (CLI)
Implementace uživatelského vstupu (`input`), kde si uživatel sám definuje parametry hledání.

**Ukázka workflow:**
1.  Skript se zeptá: *"Jakou dispozici hledáte? (např. 1+kk)"*
2.  Uživatel zadá: `1+kk`
3.  Algoritmus na pozadí:
    * Vyfiltruje byty 1+kk.
    * Rozdělí je do kategorií dle $m^2$.
    * Vypočítá průměrnou tržní cenu pro každou kategorii.
    * Vypočítá odchylku ("slevu") každého inzerátu oproti průměru.
4.  **Výstup:** Program vypíše seznam URL odkazů na byty, které jsou **cenově nejvýhodnější** (nejvíce pod průměrem) v dané velikostní kategorii.

### 3. Finanční dopad
Tento nástroj by uživateli nešetřil jen čas při hledání, ale primárně **peníze**.
* Odhalení bytu, který je o 1 000 Kč levnější než tržní standard pro jeho velikost, znamená roční úsporu **12 000 Kč**.
