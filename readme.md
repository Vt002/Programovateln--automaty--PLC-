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


### 1. Základní vlastnosti, rozdělení PLC a vnitřní architektura

*Časová dotace: 10–15 minut | Úvodní orientační a opakovací úloha*

Prostudujte vnitřní blokové uspořádání programovatelného automatu na níže uvedeném schématu:

![](Vnitřní%20uspořádání%20PLC.png)

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












## Úlohy

### 1. Základní vlastnosti a rozdělení PLC

> :key: **PLC**
>
> Programovatelný logický automat
> Programovatelný logický automat. Online. In: Wikipedia: the free encyclopedia. San Francisco (CA): Wikimedia Foundation, 2024, 23. 2. 2024 v 09:19. Dostupné z: https://cs.wikipedia.org/wiki/Programovateln%C3%BD_logick%C3%BD_automat. [cit. 2024-12-28].

1. Jaký je rozdíl mezi PLC a embedded PLC? Co znamená slovo embedded obecně a u jakých dalších typů řídících jednotek se používá?

> PLC se často rozdělují podle výkonu, ale lepší je rozlišovat pro jakou oblast automatizace jsou určena.

2. Vyhledejte a zdůvodněte konkrétní typy PLC pro následující oblasti použití:
    - Automatizace rodinných domů a bytů
    - Automatizace jednoúčelových strojů
    - Automatizace hotelů a větších budov občanské výstavby
    - Automatizace výrobních linek
    - Automatizace velkých továren, elektráren, apod.

3. K nalezeným PLC dohledejte a vysvětlete následující parametry:
    - Parametry CPU
    - Velikosti a typy pamětí
    - Napájecí napětí a odběr (proud při napájecím napětí)
    - Modulární/kompaktní
    - Počty digitálních a analogových vstupů a výstupů a jejich využití
    - Další rozhraní pro komunikaci PLC s okolím (pro programování samotného PLC, tak i pro rozšíření o další moduly a zařízení)

4. V automatizační technice se setkáme s řadou dalších řídících jednotek. Zjistěte, jaké se označují následujícími zkratkami a pro jakou oblast automatizace se používají:
    - iPC
    - NC (Numerical Control)
    - MCU

### 2. Části PLC

1. U zvoleného výrobce vyhledejte v katalogu, případně e-shopu, či konfigurátoru, jednotlivé komponenty pro funkční sestavu PLC s následujícími parametry:
    - CPU 32b nebo vyšší
    - 23 DI
    - 7 AI
    - 4 DO s PWM
    - 2 DO
    - 3 reléové výstupy
    - Sběrnice s možností rozšíření o vzdálené moduly IO

<details>
    <summary> :bulb: Tip: </summary>
        Některé firmy vyrábějící modulární PLC používají tzv. konfigurátory sestav, které bývají součástí IDE a případně propojeny s e-shopem.
        Další možností, jak si usnadnit práci, je zaslat požadavek přímo firmám jako předběžnou poptávku. Získáte tak i informaci o cenách.
</details>

> :key: **CPU u PLC**
>
> Ačkoliv zkratka CPU je obecně brána jako označení mikroprocesoru, u PLC se tím obvykle myslí celý řídící modul, který v dnešní době může obsahovat více procesorů.

2. Podrobněji prozkoumejte parametry CPU 

3. Zjistěte nejen typ sběrnice, ale i její hlavní parametry. Můžete zkusit zjistit i informace o použitém protokolu.

### 3. Základní diagnostika PLC

1. Vyhledejte technický list vybraného PLC a zjistěte, co lze zjistit o PLC z kontrolek (LED na samotném PLC).

2. Vymyslete postup, jak pomocí multimetru ověřit:
    - Napětí na napájecích svorkách zdroje a PLC, případně dalších komponent
    - Zkrat mezi vodiči
    - Prohození vodičů
    - Stav DO
    - Spečené reléové kontakty

<details>
    <summary> :bulb: Tip: </summary>
        Podívejte se na štítkové údaje, případně do technických listů. Naleznete v nich některé informace potřebné pro diagnostiku.
</details>

3. Diagnostiku PLC lze provádět i pomocí programovacího prostředí (IDE). K čemu slouží následující nástroje:
    - Forced value (vnucení/uzamknutí hodnot)
    - Nástroje Watch a Monitor
    - Debugger
    - Log soubory
    - ...

<details>
    <summary> :bulb: Tip: </summary>
        Otevřete si programovací prostředí pro PLC a uvedené nástroje vyhledejte a vyzkoušejte. Je možné, že v některých prostředích některé nástroje nenaleznete, nebo pod jiným názvem (poznamenejte si tyto názvy). Případně zkuste zjistit jiné diagnostické nástroje.
</details>

### 4. Programovací jazyky pro PLC

1. Založte projekt a v něm programy v pěti jazycích (ST - Struktural Text, LD - Ladder Diagram, CFC - Continous Function Chart a dvou dalších podle vlastního výběru).

2. V každém jazyce napište a odzkoušejte program pro zpožděné vypnutí světla.

> :key: **Programovací jazyky pro PLC**
>
> Programovacími jazyky pro PLC se zabývá norma IEC 61 131-3. 


