# Database

Her finder du alt data om GeoFA database schemaet. Informationerne er delt op i to:
- Schemaer, som indeholder alle table-schemaer i JSON dokumenter.
- Meta, som er JSON dokumenter med meta-data for de enkelte udgivne temaer. Temaer er teknisk set database views, som er baseret på tabellerne nævnt ovenfor.

Det er først og fremmest meta-dokumenterne, som er interessante for dataanalytikere, udviklere, GIS mv. Disse afspejler temaerne som de fremstår i de forskellige services, herunder
WFS og SQL. De indeholder feltstruktur, udfaldsrum for felter, titler, beskrivelser mv. Hvis man er interesseret i constraints og indekser på de enkelte felter,
fremgår de kun af schemaerne.
Ved ændringer af schemaer og meta-data bliver dette repository opdateret. Commit message vil indeholde oplysninger om typen af opdateringen, fx om det er en egentlig schema-ændring
i databasen eller om det blot er fx en titel, som er blevet ændret.
Commits til dette repository bliver automatisk skabt af GeoFA, så der er ingen mulighed for at lave pull requests.

## Hvor finder jeg hvad?

| Mappe / fil | Indhold |
|---|---|
| [`_data/meta/`](https://github.com/GeoDanmarkGeoFA/Database/tree/main/_data/meta) | **Meta-dokumenter** – ét pr. udgivet tema, fx `fkg.t_5800_fac_pkt.json`. Indeholder **alle** felter, som temaet har i WFS og SQL – både de generelle og de temaspecifikke – samt titel, formål, registreringsvejledning mv. Filer, der starter med `fkg.hist_`, er historik-udgaver af temaerne. |
| [`_data/fkg/tables/`](https://github.com/GeoDanmarkGeoFA/Database/tree/main/_data/fkg/tables) | **Schemaer** for tabellerne i databasen, inkl. constraints og indekser. |
| ↳ `t_<temakode>_<navn>_t.json` | Tabellen for ét tema, fx `t_5800_fac_pkt_t.json`. Indeholder **kun de temaspecifikke felter** – se nedenfor. |
| ↳ [`generel.json`](https://github.com/GeoDanmarkGeoFA/Database/blob/main/_data/fkg/tables/generel.json) | **De generelle felter**, som er fælles for alle temaer. |
| ↳ `d_<temakode>_<navn>.json` | Kodelister (udfaldsrum), der kun bruges i ét tema, fx `d_5800_facilitet.json` med alle facilitetstyper. |
| ↳ `d_basis_<navn>.json` | Kodelister, der bruges på tværs af temaer, fx `d_basis_status.json`. |
| [`pages/`](https://github.com/GeoDanmarkGeoFA/Database/tree/main/pages) | Skabeloner til dokumentationssiderne på [geodanmarkgeofa.github.io/Database](https://geodanmarkgeofa.github.io/Database/). |

## Hvorfor viser tabel-schemaerne kun de temaspecifikke felter?

I databasen er hvert tema delt i to tabeller:

- **`generel`** indeholder de felter, som alle temaer har til fælles (se tabellen herunder).
- **`t_<temakode>_<navn>_t`** indeholder kun de felter, der er særlige for temaet.

De to tabeller er koblet sammen via feltet `versions_id`. Det udgivne tema – det, du henter via WFS eller SQL – er et view, der samler begge dele. Derfor:

- **I `_data/fkg/tables/t_…_t.json`** ser du kun de temaspecifikke felter. De generelle felter står i [`generel.json`](https://github.com/GeoDanmarkGeoFA/Database/blob/main/_data/fkg/tables/generel.json).
- **I `_data/meta/fkg.t_….json`** ser du alle felter samlet, præcis som temaet fremstår i WFS og SQL.

Udover felterne fra de to tabeller tilføjer viewet nogle læsevenlige felter med tekstværdier, fx `temakode`, `temanavn`, `kommunekode`, `cvr_navn`, `oprindelse`, `status` og `offentlig`. De står også i meta-dokumentet.

### De generelle felter

| Felt | Indhold |
|---|---|
| `versions_id` | Unikt id for denne version af objektet. Kobler de generelle og de temaspecifikke felter sammen |
| `objekt_id` | Entydigt id for objektet. Er det samme for alle versioner af objektet |
| `systid_fra` | Hvornår denne version af objektet blev gyldig. Sættes af systemet |
| `systid_til` | Hvornår denne version af objektet holdt op med at være gyldig. Sættes af systemet |
| `oprettet` | Hvornår objektet første gang blev oprettet. Sættes af systemet |
| `cvr_kode` | CVR-nummer på den ansvarlige myndighed (kodeliste: `d_basis_ansvarlig_myndighed`) |
| `bruger_id` | Brugeren, der har oprettet eller ændret objektet. Sættes af systemet |
| `oprindkode` | Objektets oprindelse (kodeliste: `d_basis_oprindelse`) |
| `statuskode` | Objektets status (kodeliste: `d_basis_status`) |
| `off_kode` | Hvem der må se objektet (kodeliste: `d_basis_offentlig`) |
| `noegle` | Henvisning til objektet i et andet system |
| `note` | Frit bemærkningsfelt |

Den fulde beskrivelse af hvert felt står i [`generel.json`](https://github.com/GeoDanmarkGeoFA/Database/blob/main/_data/fkg/tables/generel.json).

## Den nemmeste måde at se alle felter for ét tema

Hvert tema har en dokumentationsside på [geodanmarkgeofa.github.io/Database](https://geodanmarkgeofa.github.io/Database/), fx [5800_fac_pkt](https://geodanmarkgeofa.github.io/Database/pages/5800_fac_pkt). Under *Felter i udstillings-view* står alle felter samlet, og de generelle felter er markeret med rød.

---

<sub>Del af [GeoDanmarkGeoFA](https://github.com/GeoDanmarkGeoFA) · [Om GeoFA](https://confluence.kds.dk/pages/viewpage.action?pageId=191398876) · Spørgsmål: [geofa@geodanmark.dk](mailto:geofa@geodanmark.dk)</sub>
