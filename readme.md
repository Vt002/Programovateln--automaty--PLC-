[Co dodělat ]: #
[nic ]: #

# Programovatelné automaty (PLC)

$${\color{#FFA500}E10 \space \color{Gold}S21 \space \color{#4682B4}A5}$$

## Cíl
- **Kategorizovat a porovnat** programovatelné automaty (PLC) podle aplikační oblasti, konstrukčního provedení (kompaktní, modulární na DIN lištu, distribuované I/O ostrovy) a odlišit je od embedded PLC, iPC, NC a mikrokontrolérů (MCU).
- **Analyzovat vnitřní hardwarovou architekturu PLC** (CPU, paměťové oblasti Flash/RAM/Retain, procesní obraz vstupů a výstupů PAE/PAA, sběrnice) na bázi ekosystému **Tecomat Foxtrot / TC800** a navrhnout dimenzování napájení a ochranu výstupních obvodů.
- **Sestavit a navrhnout** funkční hardwarovou konfiguraci modulárního PLC z reálných katalogů a technické dokumentace firmy **Teco a.s.** (včetně rozšiřujících modulů DI/DO/AI/AO a komunikačních linek TCL2 / CIB) na základě technologické I/O bilance s uvážením projektové rezervy.
- **Provést systematickou diagnostiku a troubleshooting** logického automatu s využitím provozních LED indikátorů, měřicích postupů digitálním multimetrem i softwarových nástrojů vývojového prostředí **Teco Mosaic** (Inspektor, Watch, Force, systémový log).
- **Implementovat a optimalizovat** řídicí algoritmus v normovaných jazycích dle **ČSN EN 61131-3** v prostředí Mosaic (**ST – Strukturovaný text** se stavovým automatem a **LD – Liniové schéma**) s aplikací standardních funkčních bloků časovačů (TON, TOF).
- **Zhodnotit a obhájit** volbu řídicího systému z hlediska celkových nákladů na vlastnictví (**TCO**), životního cyklu, dlouhodobé dostupnosti servisu a zásad funkční bezpečnosti (**Safety**).

## Ověření cílů

Programovatelné automaty (PLC)

1) Základní vlastnosti, rozdělení: podle použití, velikosti a podle modulárnosti
2) Popis částí PLC
3) Základní diagnostika PLC
4) Programovací prostředí a programovací jazyky pro PLC


---
## Úlohy


### 1. Základní vlastnosti a rozdělení PLC

*Časová dotace: 10–15 minut | Úvodní orientační a opakovací úloha*

1. **Rozdělení PLC podle oblasti nasazení:**
   Doplňte do tabulky vhodnou kategorii a konkrétní zástupce vámi vybraných výrobců. Jako vzor poslouží vyplněný řádek pro *Domovní automatizaci a budovy*:

| Oblast nasazení                                           | Vhodná koncepce PLC (kompaktní / modulární / embedded)            | Typický zástupce z portfolia Teco (nebo ekvivalent)                   | Klíčové technické parametry (sběrnice, I/O, napájení, krytí)                                                                                     |
| :-------------------------------------------------------- | :---------------------------------------------------------------- | :-------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------- |
| **Automatizace domů a budov (HVAC, Smart Home) - VZOR**   | **Kompaktní PLC na DIN lištu s integrovanou instalační sběrnicí** | **Tecomat Foxtrot 2** (např. centrální jednotka CP-2007 nebo CP-2000) | Dvě dvoudrátové sběrnice CIB (Common Installation Bus) s napájením prvků po sběrnici, LAN Ethernet, webserver, nízká spotřeba, relé 230 V / 16 A |
| **Automatizace jednoúčelových strojů a linek**            | `...`                                                             | `...` *(např. Tecomat TC800)*                                         | `...`                                                                                                                                            |
| **Rozsáhlé procesní řízení a velké energetické celky**    | `...`                                                             | `...`                                                                 | `...`                                                                                                                                            |
| **Strojní zařízení s vestavěnou elektronikou (Embedded)** | `...`                                                             | `...` *(např. OEM deskové jednotky Teco řady Foxtrot)*                | `...`                                                                                                                                            |

2. **PLC vs. Embedded PLC a další řídicí jednotky:**
   - Jaký je zásadní rozdíl mezi standardním průmyslovým PLC v krabičce na DIN lištu a tzv. **Embedded PLC**? Co obecně vyjadřuje pojem *embedded* a v jakých dalších typech zařízení se s ním setkáváme?
     - *Vaše vysvětlení:* `...`

   - Doplňte do přehledové tabulky význam zkratek a oblast jejich nasazení:

| Zkratka      | Co zkratka znamená (anglicky / česky) | Pro jakou oblast řízení se primárně využívá                        | Typický operační / řídicí systém                 |
| :----------- | :------------------------------------ | :----------------------------------------------------------------- | :----------------------------------------------- |
| **iPC**      | Industrial PC / Průmyslové PC         | Vizualizace SCADA, řízení rozsáhlých technologických uzlů, SoftPLC | Průmyslový OS (Windows IoT Enterprise, Linux RT) |
| **NC / CNC** | `...`                                 | `...`                                                              | `...`                                            |
| **MCU**      | `...`                                 | `...`                                                              | `...` *(bare-metal bez OS nebo RTOS)*            |
| **SoC**      | System on a chip                      | `...`                                                              | `...`                                            |

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **PLC (Programmable Logic Controller):** Průmyslový mikropočítačový systém určený pro deterministické řízení technologických procesů v reálném čase. Vyznačuje se vysokou odolností proti rušení, modulární koncepcí a cyklickým vykonáváním programu.
	 Programovatelný logický automat. In: *Wikipedie: otevřená encyklopedie* [online]. San Francisco (CA): Wikimedia Foundation, 2024, 23. 2. 2024 v 09:19 [cit. 2026-09-20]. Dostupné z: https://cs.wikipedia.org/wiki/Programovateln%C3%BD_logick%C3%BD_automat
> - **Embedded PLC:** Řídicí automat realizovaný jako deska plošných spojů pro přímé zabudování do stroje (OEM aplikace, např. tepelná čerpadla, kompresory) bez vnějšího krytu.

<details>
<summary> :bulb: Tip k porovnání architektur: </summary>
<p>Zatímco jednočipový mikrokontrolér (MCU) obsahuje procesor, paměti i periferie na jediném křemíkovém čipu a vyžaduje návrh desky plošných spojů, průmyslové PLC již v sobě integruje galvanické oddělení vstupů a výstupů, přepěťové ochrany, stabilizátory napětí a certifikované krytí pro montáž do elektrického rozváděče na DIN lištu.</p>
</details>

:star2: **Bonusová otázka k úloze 1:**
Proč programátor v prostředí pro PLC (např. Teco Mosaic) nesmí ve standardní programové smyčce použít nekonečný blokující cyklus (nekonečná smyčka/infinite loop, např. `WHILE True DO ...`), se kterým se lze setkat u programování desktopových aplikací na PC a programování MCU? Jaký interní bezpečnostní mechanismus procesoru PLC by v takovém případě zasáhl?

*Vaše odpověď:*
`...`

---

### 2. Části PLC, paměti, procesní obraz a provozní vlastnosti

*Časová dotace: max. 15 minut | Propojení parametrů, signálů, reálného času a odolnosti*

Prostudujte vnitřní blokové uspořádání programovatelného automatu na níže uvedeném schématu:

![](Vnitřní%20uspořádání%20PLC.png)

1. **Vnitřní uspořádání a paměťové oblasti PLC:**
   - Podle výše uvedeného blokového schématu a technické dokumentace PLC Teco doplňte funkci jednotlivých paměťových bloků v procesorové jednotce:
     - **Flash paměť (uživatelský program):** 
       - Je volatilní (energeticky závislá)? `[Ano / Ne]`
       - Co přesně se do ní nahrává z prostředí Mosaic: `...`
     - **Datová paměť RAM:** 
       - Je volatilní? `[Ano / Ne]`
       - K čemu slouží během cyklu PLC: `...`
     - **Remanentní paměť (Retain / zálohovaná RAM):** 
       - Jakým způsobem je zálohována při výpadku napájení v modulech Tecomat Foxtrot (akumulátor / superkondenzátor / paměť FRAM): `...`
       - Uveďte typický příklad proměnné, která musí být uložena v remanentní paměti: `...`

2. **Cyklický princip činnosti PLC a procesní obraz:**
	
	<img src="attachments/Pasted%20image%2020260928084647.png" width="313" alt="">
	
	Obr. převzatý z: TECO A.S. *PROGRAMOVATELNÉ AUTOMATY TECOMAT FOXTROT 2*. Kolín: Teco a.s., 2026. Dostupné také z: https://wiki.tecomat.cz/
	
   - Popište 3 základní fáze jednoho pracovního cyklu PLC (**Scan Cycle**):
     - *Fáze 1 (Čtení vstupů):* `...` *(načtení stavu svorek do Procesního obrazu vstupů – PII)*
     - *Fáze 2 (Vykonání programu):* `...` *(výpočet logických a matematických operací)*
     - *Fáze 3 (Zápis výstupů):* `...` *(přenesení Procesního obrazu výstupů – PIQ na fyzické svorky)*
   - Proč procesor PLC nečte fyzické svorky průběžně během výpočtu programu na každém řádku kódu, ale pracuje výhradně s procesním obrazem?
     - *Vysvětlení:* `...`

3. **Napájení, signály a elektrická odolnost:**
   - Jaké je jmenovité napájecí napětí centrálních jednotek a rozšiřujících modulů systému Tecomat Foxtrot?
     - Označte správné možnosti: 
	     `[ ] DC`
	     `[ ] AC`
	     `[ ] 5 V`
		 `[ ] 12 V`
		 `[ ] 24 V`
		 `[ ] 48 V` 
		 `[ ] 230 V`
   - Jaký je princip fungování a hlavní výhoda **galvanického oddělení** vstupních obvodů (pomocí optočlenů) u průmyslového PLC?
     - *Odpověď:* `...`
   - Proč musí být paralelně k cívce stejnosměrného elektromagnetického ventilu nebo stykače spínaného polovodičovým tranzistorovým výstupem PLC zapojena ochranná zhášecí dioda (tzv. flyback dioda/freewheeling diode)?
     - *Vysvětlení:* `...`

4. **Komunikační rozhraní a systémové sběrnice Teco:**
   - Doplňte do tabulky parametry sběrnic používaných u systému Tecomat Foxtrot (využijte [wiki.tecomat.cz](https://wiki.tecomat.cz/)):

| Název sběrnice                         | Typická fyzická vrstva / kabeláž                        | K čemu sběrnice slouží                                | Maximální dosah / zakončení (terminace)                      |
| :------------------------------------- | :------------------------------------------------------ | :---------------------------------------------------- | :----------------------------------------------------------- |
| **TCL2** *(Teco Communication Line 2)* | RS-485, stíněný kroucený pár (např. kabel SYKFY, PCEHY) | Připojení rychlých periferních modulů na DIN lištu    | Až stovek metrů; na obou koncích zakončovací odpor cca 120 Ω |
| **CIB** *(Common Installation Bus)*    | `...`                                                   | `...`                                                 | `...`                                                        |
| **Ethernet (ETH)**                     | 100Base-TX, konektor RJ45 (kabel UTP/STP Cat 5e/6)      | `...` *(programování Mosaic, Web server, Modbus TCP)* | Max. 100 m na segment                                        |

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Procesní obraz (Process Image):** Paměťová oblast v RAM kontroléru, která uchovává snímek stavu všech fyzických vstupů pořízený na počátku cyklu a připravený stav výstupů pro zápis na konci cyklu. Zajišťuje časovou konzistenci dat během zpracování algoritmu.
> - **Sběrnice CIB (Common Installation Bus):** Dvouvodičová instalovaná sběrnice vyvinutá firmou Teco, která umožňuje po jediném nestíněném krouceném páru vodičů přenášet napájení připojených prvků i obousměrná komunikační data (volná topologie: strom, hvězda, liniová).
> - **Sběrnice TCL2:** Rychlá systémová sběrnice na bázi průmyslové linky RS-485 pro vysokorychlostní komunikaci centrální jednotky Foxtrot s rozšiřujícími I/O moduly v rozváděči.
>
> *Bibliografické citace:*
> - TECO A.S. *Instalační sběrnice CIB: Systémový manuál*. Kolín: Teco a.s., 2023. Dostupné také z: https://wiki.tecomat.cz/
> - ČESKÝ NORMALIZAČNÍ INSTITUT. *ČSN EN 61131-2 ed. 3 (18 0080) Programovatelné řídicí jednotky - Část 2: Požadavky na zařízení a zkoušky*. Praha: Úřad pro technickou normalizaci, metrologii a státní zkušebnictví, 2018. Třídící znak 180080.

<details>
<summary> :bulb: Tip k napájení a sběrnicím: </summary>
<p>U sběrnice CIB jsou periferní senzory a aktory (např. nástěnné ovladače, pohybová čidla PIR, stmívače) napájeny přímo stejnosměrným napětím ze sběrnice, což dramaticky snižuje množství kabeláže v objektu. Naopak sběrnice TCL2 je striktně určena pro rychlý přenos I/O dat mezi moduly v rozváděči a vyžaduje správné impedanční zakončení 120 Ω.</p>
</details>

:star2: **Bonusová otázka k úloze 2:**
V jakých jednotkách udává norma **ČSN EN 60529** odolnost proti vniknutí pevných těles a kapalin (kód IP) a jaké minimální krytí musí mít centrální jednotka PLC určená pro montáž na DIN lištu do rozváděče (např. Tecomat Foxtrot v provedení IP20) ve srovnání s decentralizovaným senzorem na venkovním potrubí? Ověřte i správnost označení normy.

*Vaše odpověď:*
`...`

---

### 3. Inženýrská konfigurace funkční sestavy PLC Teco a proudová bilance

*Časová dotace: 25–30 minut | :bangbang: Klasifikovaná inženýrská úloha na známky*

Jste v roli projektanta automatizačních systémů. Zákazník požaduje navrhnout funkční modulární řídicí systém postavený na platformě **Tecomat Foxtrot 2** (případně Tecomat TC800) od společnosti **Teco a.s.** (využijte oficiální dokumentaci [wiki.tecomat.cz](https://wiki.tecomat.cz/) nebo konfigurátor Teco) pro zadanou technologickou linku.

#### Požadovaná I/O bilance technologie:
- **23× digitální vstup (DI 24 V DC)** – indukční koncové spínače a optické snímače polohy.
- **7× analogový vstup (AI)** – z toho 5× proudový signál 4–20 mA (tlaková čidla a hladinoměry) a 2× přímé měření teploty odporovým čidlem PT100/Pt1000.
- **4× rychlý tranzistorový výstup s PWM (DO PWM 24 V DC / 0,5 A)** – pro přesné dávkovací ventily a regulaci výkonu.
- **2× standardní tranzistorový výstup (DO 24 V DC)** – signalizační sloupky a stavové majáky.
- **3× reléový výstup (DO relé, bezpotenciálový kontakt min. 230 V AC / 3 A)** – spínání stykačů topných těles.
- **Systémová sběrnice** s možností připojení dalších decentralizovaných periferií (např. TCL2 nebo CIB).

#### Ukázka možného přístupu k řešení:
> *Příklad postupu návrhu sestavy Teco:*
> 1. *Základní centrální jednotka:* Volba např. **CP-2007** (obsahuje 14 univerzálních vstupů konfigurovatelných jako DI nebo AI pro odporová čidla i proud 4–20 mA, 10 reléových výstupů a 2 analogové výstupy s funkcí PWM, vestavěný Ethernet a sběrnice TCL2 + CIB).
> 2. *Rozšíření chybějících I/O po systémové sběrnici TCL2:* Přidání periferií z řady Foxtrot:
>    - Např. modul binárních vstupů **IB-1301** (12× DI 24 V DC) pro pokrytí zbývajících binárních vstupů.
>    - Např. modul binárních výstupů **OS-1401** (12× polovodičový výstup 24 V DC) pro rychlé výstupy a PWM.
> 3. *Kalkulace rezervy:* Přepočet celkových kapacit na požadavek technologie (minimální rezerva 15–20 %).

#### Váš úkol:

1. **Konfigurace komponent z katalogu Teco (vyplňte návrhovou tabulku):**

| Pozice | Typ modulu / komponenta                     | Přesné typové označení Teco           | Objednací číslo (Part No.) | Pokryté vstupy a výstupy (DI / DO / AI / PWM / relé) |
| :----- | :------------------------------------------ | :------------------------------------ | :------------------------- | :--------------------------------------------------- |
| **1**  | Spínaný napájecí zdroj 24 V DC na DIN lištu | `...` *(např. PS2-60/27)*             | `...`                      | Napájení centrály a senzorů 24 V DC                  |
| **2**  | Centrální jednotka (CPU)                    | `...` *(např. CP-2007 nebo CP-2005)*  | `...`                      | `...`                                                |
| **3**  | Rozšiřující modul 1 (sběrnice TCL2)         | `...` *(např. IB-1301)*               | `...`                      | `...`                                                |
| **4**  | Rozšiřující modul 2 (sběrnice TCL2)         | `...` *(např. OS-1401)*               | `...`                      | `...`                                                |
| **5**  | Případný další submodul / zakončení         | `...` *(např. zakončovací člen TCL2)* | `...`                      | `...`                                                |

2. **Vyhodnocení I/O bilance a projektové rezervy:**
   - Doplňte počty a vypočítejte výslednou rezervu pro budoucí rozšíření linky:

| Typ signálu | Požadavek linky | Celková kapacita navržené sestavy Teco | Rezerva (počet volných svorek) | Výsledná rezerva v % |
| :--- | :--- | :--- | :--- | :--- |
| **Digitální vstupy (DI)** | 23 | `...` | `...` | `... %` |
| **Analogové vstupy (AI: 4–20mA, PT)**| 7 | `...` | `...` | `... %` |
| **Tranzistorové výstupy (DO tranzistor + PWM)** | 6 (4 PWM + 2 DO)| `...` | `...` | `... %` |
| **Reléové výstupy (DO relé 230 V)**| 3 | `...` | `...` | `... %` |

3. **Proudová bilance a dimenzování napájecího zdroje 24 V DC:**
   - Vypočtěte proudovou spotřebu sestavy na hladině 24 V DC:
     - Vlastní příkon CPU a rozšiřujících modulů (dle datasheetů Teco): `...` W / 24 V = `...` A
     - Odběr napájených snímačů DI (23 snímačů × cca 10 mA): `...` A
     - Odběr zátěží na tranzistorových výstupech se soudobostí $k_s = 0{,}7$: `...` A
     - **Celkový špičkový proud:** `...` A
     - Jaký napájecí zdroj 24 V DC z nabídky Teco / Mean Well zvolíte (včetně doporučené rezervy 25 %)?
       - *Zvolený zdroj a jmenovitý proud:* `...`
    - Navrhněte alternativu tohoto zdroje od jiného výrobce: `...`

> **Kritéria hodnocení úlohy 3 (bodování na známky):**
> - :bangbang: **Správnost technického výběru a kompatibilita systému Teco (40 %):** Všechny zvolené moduly existují v portfoliu Teco a.s., jsou vzájemně kompatibilní (sběrnice TCL2/CIB) a plně pokrývají zadané signály včetně specifických požadavků (odporové měření PT100/Pt1000, 4–20 mA a rychlé výstupy PWM).
> - :bangbang: **I/O bilance a projektová rezerva (30 %):** Korektní výpočet rezervy pro budoucí rozšíření linky (doporučená rezerva min. 15–20 %).
> - :bangbang: **Dimenzování napájecího zdroje (30 %):** Správná kalkulace proudové zátěže 24 V DC se započtením soudobosti a zdůvodnění volby zdroje.

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Univerzální vstupy (UI / AI / DI):** Konstrukční řešení vstupních obvodů Teco Foxtrot, kdy lze jednotlivé svorky softwarově v konfigurátoru nastavit buď jako bezpotenciálový binární vstup (kontakty, snímače), nebo jako analogový vstup (napěťový 0–10 V, proudový 4–20 mA, či přímé měření teplotních odporových čidel Pt1000/Ni1000).
> - **Soudobost zátěží ($k_s$):** Statistický koeficient (zpravidla 0,7 až 0,8), který zohledňuje, že v reálném provozu nikdy nesepnou všechny akční členy a ventily v tentýž okamžik na plný proudový výkon.
>
> *Bibliografické citace dle normy ČSN ISO 690:*
> - TECO A.S. *Základní modul CP-2007: Technický list a popis svorek* [online]. Kolín: Teco a.s., 2024 [cit. 2026-09-20]. Dostupné z: https://wiki.tecomat.cz/index.php?title=CP-2007
> - TECO A.S. *Periferní moduly systému Foxtrot na sběrnici TCL2* [online]. Kolín: Teco a.s., 2023 [cit. 2026-09-20]. Dostupné z: https://wiki.tecomat.cz/

<details>
<summary> :bulb: Tip k analogovým vstupům u Teco Foxtrot: </summary>
<p>Prozkoumejte zapojení univerzálních vstupů u jednotek CP-2007: Vstupy AI0 až AI5 umožňují přímé připojení odporových čidel (Pt1000, Ni1000) i proudových smyček 4–20 mA (pomocí interních přepojitelných bočníků nebo konfigurace). Tím ušetříte náklady na speciální externí převodníky.</p>
</details>

:star2: **Bonusová otázka k úloze 3:**
Jaký je princip zapojení odporového snímače teploty (např. Pt1000) a proč je pro přesné měření na velké vzdálenosti vhodnější použít snímač Pt1000 než Pt100, pokud máme k dispozici pouze standardní 2vodičové vedení?

*Vaše odpověď:*
`...`

---

### 4. Provozní diagnostika PLC, měření multimetrem a troubleshooting v IDE Mosaic

*Časová dotace: 25–30 minut | :bangbang: Klasifikovaná inženýrská úloha na známky*

Jste v roli servisního inženýra na lince řízené automatem **Tecomat Foxtrot**. Linka nečekaně zastavila cyklus. Na operátorském panelu svítí alarm „PORUCHA ČERPÁNÍ – TLAKOVÁ ZTRÁTA“, na centrální jednotce PLC bliká červená LED kontrolka `ERR` a čerpadlo nereaguje na povely.

#### Příklad vzorového diagnostického protokolu (dobrá servisní praxe):
> *Vzorový postup při nefunkčním digitálním snímači:*
> 1. *Kontrola stavové LED na svorce modulu:* Vizuální pohled na LED vstupu DI2 – LED nesvítí, přestože je válec v poloze sepnutí.
> 2. *Měření digitálním multimetrem:*
>    - Nastaven rozsah měření DC napětí (200 V DC).
>    - Černý hrot COM připojen na nulový potenciál (svorka GND / 0 V), červený hrot na svorku napájení snímače (+24 V DC) -> naměřeno 24,2 V DC (napájení senzoru je v pořádku).
>    - Červený hrot přemístěn na signálovou svorku vstupu DI2 -> naměřeno 0,3 V DC (logická 0).
> 3. *Závěr měření:* Vadný spínací tranzistor v indukčním senzoru nebo přerušený signálový kabel v energetickém řetězu linky.

#### Váš úkol:

1. **Diagnostika podle stavových kontrolek (LED) a displeje centrální jednotky Teco:**
   - Doplňte do tabulky význam signalizace na čelním panelu centrální jednotky Foxtrot (CP-2000 / CP-2091):

| Signalizační prvek na PLC | Stav (barva / svit)     | Provozní význam (co stav znamená pro servisního technika)   |
| :------------------------ | :---------------------- | :---------------------------------------------------------- |
| **RUN**                   | Zelená, trvale svítí    | Uživatelský program v PLC běží v normálním cyklickém režimu |
| **HALT / STOP**           | Žlutá / oranžová, svítí | `...`                                                       |
| **ERR / FAULT**           | Červená, bliká          | `...`                                                       |
| **ERR / FAULT**           | Červená, trvale svítí   | `...`                                                       |
| **ETH (Link/Act)**        | Zelená/žlutá, bliká     | `...`                                                       |

2. **Troubleshooting v elektrickém zapojení pomocí digitálního multimetru:**
   - Upevněte na DIN lištu PLC a podle parametrů (viz technický list) vyberte vhodný zdroj napájení.
   - Zapojte napájení PLC (než zapojíte 230 V AC zapojení proměřte).
   - Popište přesný a bezpečný metodický postup (kam připojíte měřicí hroty, jaký rozsah a veličinu na multimetru nastavíte; alespoň některé varianty bezpečně nasimulovat na PLC a zdroji, nejspíš budete potřebovat navázat komunikaci s programovacím prostředím, založit program a HW konfiguraci, abyste mohli ovládat výstupy a číst vstupy):
     - **A. Detekce tvrdého zkratu mezi svorkami +24 V DC a 0 V (GND) před zapnutím jističe:**
       - *Postup:* `...` *(pozor: zařízení musí být zcela bez napětí, měření impedance/kontinuity)*
     - **B. Ověření spečených kontaktů výstupního relé DO při vypnutém napájení řízení:**
       - *Postup:* `...`
     - **C. Diagnostika napájecího zdroje a napájení PLC (ověření správného napětí 24 V DC):**
	   - *Postup:* `...` *(zařízení pod napětím, měření stejnosměrného napětí – hroty na svorkách zdroje / PLC napájení +24 V a 0 V, vhodný rozsah DC napětí, kontrola ripple/stability)*
	- **D. Diagnostika vstupů a výstupů PLC (ověření napětí / kontinuity na DI/DO):**
       - *Postup:* `...` *(podle typu – měření napětí na aktivovaném vstupu/výstupu nebo kontinuity/impedance při vypnutém napájení, hroty na příslušných svorkách I/O a referenční GND)*
    - **E. Měření napětí 230 V AC pomocí DC rozsahu (!!! Tento úkol provádějte pouze s vyučujícím !!!):**
	   - *Postup:* `...` *(zařízení pod napětím, multimetr nastaven na rozsah **1000 V DC**, hroty bezpečně připojeny na fázový a nulový vodič / L–N. Multimetr měří střední hodnotu, proto ukáže přibližně **0 V**.)*
	   - *Jaké riziko z toho vyplývá při následné práci na zařízení:* `...`

3. **Diagnostika v programovacím prostředí Teco Mosaic:**
   - K jakému diagnostickému účelu slouží níže uvedené nástroje prostředí Mosaic a jaká jsou jejich bezpečnostní rizika:
     - **Nástroj Watch / Sledování proměnných (Inspektor):**
       - *Účel:* `...`
     - **Funkce Force (Vnucení hodnoty vstupu / výstupu):**
       - *Co přesně funkce provede:* `...`
       - :warning: **Provozní a bezpečnostní riziko funkce Force:** Proč je použití funkce Force během ostrého provozu s přítomností lidské obsluhy zakázáno? `...`
     - **Systémový diagnostický log (Chybový protokol PLC v Mosaicu):**
       - *Jaké informace v něm technik vyčte:* `...` *(např. restart CPU, výpadek sběrnice TCL2, chyba dělení nulou)*

> **Kritéria hodnocení úlohy 4 (bodování na známky):**
> - :bangbang: **Odborná správnost interpretace stavů a hlášení PLC (30 %):** Bezchybné vysvětlení indikace LED (RUN, HALT, ERR) a práce s diagnostickým hlášením v prostředí Mosaic.
> - :bangbang: **Metodika a bezpečnost měření multimetrem (35 %):** Správná volba měřicí funkce (napětí DC vs. odpor/kontinuita vs. proud v sérii), uvědomění si nutnosti odpojení napájení při měření odporu kontaktů relé.
> - :bangbang: **Bezpečnostní úroveň práce v prostředí Mosaic (35 %):** Správné vysvětlení funkce Force, jejího rozdílu oproti pouhému zápisu proměnné a kritické zhodnocení rizik nekontrolovaného sepnutí pohonů.

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Prostředí Mosaic:** Integrované vývojové prostředí (**IDE**) pro programování, konfiguraci, simulaci a online diagnostiku řídicích systémů Tecomat.
> - **Vnucení proměnné (Force):** Diagnostický režim, kdy je hodnota proměnné v procesním obrazu PLC natvrdo uzamčena na zvolenou úroveň (log. 0 nebo 1) bez ohledu na reálný stav fyzické svorky či algoritmus programu.
> - **Spečení kontaktů relé (Contact Welding):** Poruchový stav elektromechanického relé způsobený elektrickým obloukem při vypínání indukční zátěže nebo proudovým nárazem, kdy dojde k natavení a trvalému mechanickému spojení spínacích kontaktů.
>
> *Bibliografické citace dle normy ČSN ISO 690:*
> - TECO A.S. *Vývojové prostředí Mosaic: Uživatelská příručka a diagnostika* [online]. Kolín: Teco a.s., 2024 [cit. 2026-09-20]. Dostupné z: https://wiki.tecomat.cz/

<details>
<summary> :bulb: Tip pro měření proudové smyčky 4–20 mA: </summary>
<p>Pamatujte, že proud se měří <strong>v sérii</strong> s obvodem (musíte rozpojit svorku a multimetr zapojit jako ampérmetr do cesty proudu), zatímco napětí se měří <strong>paralelně</strong>. Pokud má PLC interní bočník (např. 250 Ω nebo 500 Ω), lze proud měřit nepřímo jako úbytek napětí na tomto odporu bez nutnosti rozpojovat vodiče (dle Ohmova zákona).</p>
</details>

:star2: **Bonusová otázka k úloze 4:**
Co se stane v řídicím systému Tecomat Foxtrot, pokud na sběrnici TCL2 omylem nastavíte dvěma různým periferním modulům stejnou hardwarovou adresu (např. otočným přepínačem adresy), a jak tuto kolizi indikuje centrální jednotka v diagnostice?

*Vaše odpověď:*
`...`

---

### 5. Simulace řídicího programu v prostředí Teco Mosaic: od teorie k praxi

*Časová dotace: 25–30 minut | :bangbang: Klasifikovaná inženýrská úloha na známky*

Prostředí **Teco Mosaic** umožňuje úplnou softwarovou simulaci cyklického běhu PLC bez fyzického hardware. Simulátor zprostředkovává procesní obraz vstupů a výstupů, odpočítávání časovačů i sledování proměnných v reálném čase – stejně jako skutečný automat. Tato úloha vás provede celým pracovním tokem: od založení projektu přes napsání řídicího algoritmu až po jeho ověření v simulátoru.

#### Technologický scénář – čerpací stanice odpadních vod:

Naprogramujte a simulujte v prostředí Teco Mosaic řízení jednoduché **čerpací stanice** s následující logikou:

| Signál | Typ | Popis |
| :--- | :--- | :--- |
| `di_HladinaDolni` | DI (NC, log. 1 = hladina nad min.) | Plovákový spínač spodní hladiny – chrání čerpadlo před chodem nasucho |
| `di_HladinaPump` | DI (NO, log. 1 = hladina dosáhla zapínací úrovně) | Plovákový spínač zapínací hladiny |
| `di_HladinaHavar` | DI (NO, log. 1 = přepad) | Plovákový spínač havarijního přepadu |
| `di_Porucha` | DI (NC, log. 1 = bez poruchy) | Termistorové ochranné relé motoru |
| `do_Cerpadlo` | DO | Výstup stykače hlavního čerpadla |
| `do_Alarm` | DO | Výstup siréna/maják – havarijní stav |

**Požadovaná logika:**
- Čerpadlo se **automaticky spustí**, jakmile hladina dosáhne zapínací úrovně (`di_HladinaPump = TRUE`) **a zároveň** je hladina nad minimem (`di_HladinaDolni = TRUE`, NC kontakt) **a zároveň** není žádná porucha motoru (`di_Porucha = TRUE`, NC kontakt).
- Čerpadlo **automaticky zastaví**, jakmile hladina klesne pod zapínací úroveň (`di_HladinaPump = FALSE`). Implementujte **časové zpoždění vypnutí 10 s** (časovač `TOF`), aby se zabránilo krátkodobým cyklickým startům (tzv. taktování čerpadla).
- Při havarijním přepadu (`di_HladinaHavar = TRUE`) se **okamžitě spustí alarm** `do_Alarm` a stav se zapíše do remanentní proměnné `b_HavarieDetekce : BOOL RETAIN`. Alarm zůstane aktivní až do ručního resetu operátorem (`btn_Reset`).
- Při výpadku ochrany motoru (`di_Porucha = FALSE`) se čerpadlo okamžitě zastaví a aktivuje se alarm.

#### Váš úkol:

1. **Postup při zakládání projektu a HW konfigurace v Mosaicu (doplňte kroky):**

   Popište, co musíte nastavit v prostředí Teco Mosaic, abyste mohli spustit simulaci, aniž byste měli připojený fyzický PLC:

   | Krok | Co v Mosaicu nastavit / zkontrolovat | Proč je tento krok nutný |
   | :--- | :--- | :--- |
   | **1. Volba cílové platformy** | V HW konfiguraci zvolit centrální jednotku, např. `...` *(např. CP-2007)* | Simulátor musí znát počet a typy I/O svorek |
   | **2. Aktivace simulátoru** | V menu `...` přepnout komunikaci z „Ethernet / přímé spojení" na `...` | Bez aktivace simulátoru by Mosaic hledal fyzické PLC na síti |
   | **3. Přiřazení proměnných k I/O svorkám** | V záložce `...` přiřadit proměnnou `di_HladinaDolni` na svorku `...` | Propojuje proměnnou v programu s fyzickým (nebo simulovaným) vstupem |
   | **4. Spuštění simulace** | Kliknout na tlačítko `...` nebo stisknout klávesovou zkratku `...` | Spustí cyklický běh programu v simulátoru (náhrada za fyzické PLC) |
   | **5. Sledování proměnných** | Otevřít okno `...` a přidat proměnné `do_Cerpadlo`, `stav`, `timer_TOF` | Umožní sledovat aktuální hodnoty proměnných v každém cyklu |

2. **Implementace řídicího algoritmu v jazyce ST:**

   Napište program pro výše popsanou logiku čerpací stanice. Použijte strukturu s remanentní proměnnou a časovačem `TOF`. Vzorová kostra programu:

```pascal
PROGRAM Prg_CerpStation
VAR
    di_HladinaDolni  : BOOL; // NC plovák – spodní hladina (log.1 = OK)
    di_HladinaPump   : BOOL; // NO plovák – zapínací hladina
    di_HladinaHavar  : BOOL; // NO plovák – havarijní přepad
    di_Porucha       : BOOL; // NC termistor (log.1 = bez poruchy)
    btn_Reset        : BOOL; // Ruční reset alarmu operátorem
    do_Cerpadlo      : BOOL; // Výstup stykače čerpadla
    do_Alarm         : BOOL; // Výstup majáku / sirény

    b_HavarieDetekce : BOOL; // RETAIN – příznak havárie přepadu (přežije výpadek)
    timer_TOF        : TOF;  // Zpoždění vypnutí čerpadla (10 s)
END_VAR

// === Doplňte implementaci logiky níže ===

// 1. Reset havarijního příznaku operátorem:
...

// 2. Detekce havarijního přepadu nebo poruchy motoru:
...

// 3. Podmínka pro spuštění čerpadla (všechny podmínky splněny):
...

// 4. Řízení výstupu čerpadla přes časovač TOF (zpoždění vypnutí 10 s):
timer_TOF(IN := ..., PT := T#10s);
do_Cerpadlo := timer_TOF.Q AND ...;

// 5. Aktivace alarmu:
do_Alarm := ...;

END_PROGRAM
```

   - *Váš doplněný kód v ST:*
```pascal
// Vložte váš kompletní program:
...
```

3. **Ověření chování programu v simulátoru Mosaic (protokol o simulaci):**

   Po spuštění simulace v Mosaicu ověřte chování programu. Popište nebo doplňte očekávané hodnoty proměnných v okně **Watch** pro každý testovací scénář:

   | Testovací scénář | Vstupní podmínky (hodnoty v simulátoru) | Očekávaný stav výstupů | Skutečně naměřeno v simulátoru |
   | :--- | :--- | :--- | :--- |
   | **Normální jímání – hladina roste** | `di_HladinaDolni=1`, `di_HladinaPump=0`, `di_Porucha=1`, `di_HladinaHavar=0` | `do_Cerpadlo=0`, `do_Alarm=0` | `...` |
   | **Spuštění čerpadla** | `di_HladinaDolni=1`, `di_HladinaPump=1`, `di_Porucha=1`, `di_HladinaHavar=0` | `do_Cerpadlo=1`, `do_Alarm=0` | `...` |
   | **Vyprázdnění – čekání TOF** | `di_HladinaDolni=1`, `di_HladinaPump=0 (právě klesl)`, vše ostatní OK | `do_Cerpadlo=1` *(ještě běží 10 s TOF)*, `timer_TOF.ET` roste | `...` |
   | **Havarijní přepad** | `di_HladinaDolni=1`, `di_HladinaPump=1`, `di_HladinaHavar=1`, `di_Porucha=1` | `do_Alarm=1`, `b_HavarieDetekce=1` | `...` |
   | **Porucha motoru (termistorové relé)** | `di_HladinaPump=1`, **`di_Porucha=0`** | `do_Cerpadlo=0`, `do_Alarm=1` | `...` |
   | **Reset alarmu** | Po stisku `btn_Reset`, porucha i přepad odstraněny | `do_Alarm=0`, `b_HavarieDetekce=0` | `...` |

4. **Propojení teorie s výsledky simulace:**
   - Proč proměnná `b_HavarieDetekce` musí být deklarována jako `RETAIN`? Co by se stalo při výpadku napájení PLC, kdyby nebyla uložena v remanentní paměti?
     - *Vaše odpověď:* `...`
   - Sledujte v okně Watch hodnotu `timer_TOF.ET` (elapsed time) během odpočtu 10 sekund. K čemu slouží tato hodnota z pohledu diagnostiky v praxi?
     - *Vaše odpověď:* `...`
   - Jakým způsobem byste pomocí funkce **Force** v Mosaicu nasimulovali výpadek ochrany motoru (`di_Porucha = FALSE`) bez fyzického přepojení svorky? Popište postup a uveďte bezpečnostní riziko:
     - *Postup Force:* `...`
     - *Bezpečnostní riziko při použití Force v ostrém provozu:* `...`

> **Kritéria hodnocení úlohy 5 (bodování na známky):**
> - :bangbang: **Správný postup práce v simulátoru Mosaic (25 %):** Správně popsané kroky pro aktivaci simulátoru, přiřazení I/O a spuštění online monitoringu (Watch).
> - :bangbang: **Správnost implementace algoritmu v jazyce ST (45 %):** Správná logika podmínek, správné použití časovače `TOF` (vstup `IN`, výstup `Q`, parametr `PT`), deklarace `RETAIN` proměnné, ošetření všech poruchových stavů.
> - :bangbang: **Protokol o simulaci – propojení teorie a praxe (30 %):** Správně vyplněné výsledky sledování ve Watch okně, věcné zdůvodnění role RETAIN paměti a bezpečnostního rizika funkce Force.

> :key: **Vysvětlení pojmů a odborné zdroje:**
> - **Simulátor Teco Mosaic:** Integrovaný software Mosaicu, který plně emuluje cyklický běh procesorové jednotky Foxtrot včetně odpočtu časovačů, práce s procesním obrazem a remanentní pamětí – bez nutnosti připojení fyzického PLC. Student tak může odladit celý program ještě před nasazením na skutečné zařízení.
> - **TOF (Timer Off-Delay / Časovač se zpožděním vypnutí):** Standardní funkční blok dle IEC 61131-3. Výstup `Q` je aktivní po celou dobu aktivního vstupu `IN`. Po deaktivaci `IN` zůstane `Q` aktivní ještě po dobu nastavenou v `PT`. Použití: zabránění krátkodobým cyklickým startům motorů (ochrana před taktováním čerpadla).
> - **RETAIN (Remanentní proměnná):** Proměnná uložená v části paměti PLC (FRAM, zálohovaná RAM nebo superkondenzátorem), jejíž hodnota přežije výpadek napájení a je dostupná ihned po opětovném startu automatu. Typické použití: počítadla kusů, příznaky havárie, nastaveníparametrů procesu.
> - **Watch / Inspektor v Mosaicu:** Okno online monitoringu proměnných. Zobrazuje aktuální hodnotu libovolné proměnné programu v každém skenu PLC. Neovlivňuje chod programu – slouží pouze ke čtení.
> - **Force (Vnucení hodnoty):** Diagnostický nástroj, který natvrdo uzamkne hodnotu proměnné v procesním obrazu na zvolenou hodnotu, nezávisle na fyzickém stavu svorky ani na algoritmu programu. V simulátoru bezpečně nahrazuje fyzické přepojení vstupní svorky.
>
> *Bibliografické citace:*
> - ČESKÝ NORMALIZAČNÍ INSTITUT. *ČSN EN 61131-3 ed. 3 (18 0080) Programovatelné řídicí jednotky - Část 3: Programovací jazyky*. Praha: Úřad pro technickou normalizaci, metrologii a státní zkušebnictví, 2014. Třídící znak 180080.

<details>
<summary> :bulb: Tip k aktivaci simulátoru v Mosaicu: </summary>
<p>V menu <strong>Projekt → Manažer projektu</strong> (nebo v liště nástrojů) přepněte komunikační mód z „Ethernet" na <strong>„Simulovaný PLC"</strong>. Poté klikněte na <strong>Přeložit a Run</strong>. Mosaic spustí virtuální PLC přímo na vašem počítači. V dolním panelu otevřete <strong>okno Data</strong> (menu Zobrazit → Data nebo Ctrl+Alt+W) a přidejte proměnné, které chcete sledovat.</p>
</details>

:star2: **Bonusová otázka k úloze 5:**
Proč se u fyzického tlačítka nouzového zastavení (E-Stop) v zapojení do vstupu PLC striktně vyžaduje rozpínací kontakt (NC), a jak se tato hardwarová skutečnost projeví v zápisu podmínky v jazyce ST (`IF NOT btn_Stop` vs. `IF btn_Stop`)? Co by se stalo při přetržení kabelu, kdyby bylo použito spínací tlačítko (NO)?

*Vaše odpověď:*
`...`

---
