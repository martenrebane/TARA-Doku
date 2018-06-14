---
permalink: Elutukse
---

# TARA elutukseteenus

Elutukse (heartbeat) otspunkt on liides, mille kaudu TARA rakendus annab reaalajas teavet rakenduse seisundi kohta.

## Sisene elutukseteenus

MFN https://e-gov.github.io/MFN/#17.3:

"Süsteemi iga eraldi paigaldatav osa peab logimisel (näiteks aadressilt heartbeat.json) väljastama masinloetaval kujul oma nime ja versiooninumbri, oluliste väliste süsteemide oleku, viimase käivitamise aja, pakendamise aja ning serveriaja."

Nõuded:

- REST otspunkt, mille poole saab pöörduda HTTP `GET` päringuga
- URL-il `https://tara.ria.ee/status` (p.o konf-tav)
- Elutukse otspunkti peab pakkuma iga eraldipaigaldatav komponent (s.t ka TARA-Client)
- Elutukse otspunkti poole hakkab pöörduma RIA monitooringurakendus
- Elutukse otspunkti hakkab kasutama ka TARA teenusehaldur
  - teenusehaldur tegutseb rakendusehalduri rollis
  - teenusehaldur pöördub otspunkti veebisirvija aadressirealt URL-iga
    - ja loeb JSON-vastust

- Otspunkt peab minimaalselt tagastama JSON-struktuuri:
  - rakenduse seisund (OK või NOK)
  - rakenduse käivitamise aeg (DateTime)
  - kaua rakendus on üleval olnud (ISO 8601 kestus, vt [nt](https://www.digi.com/resources/documentation/digidocs/90001437-13/reference/r_iso_8601_duration_format.htm)).
- Lisaks võiks elutukse pakkuda teatud määral jooksvat statistikat, lähtudes rakenduse halduse vajadustest (teostada mitmes järgus?):
  - teenuse poole pöördumiste arv
    - nt jooksva päeva, eelmise päeva jooksul
    - autentimismeetodite lõikes (2. järgus)
- Väliste teenuste (eIDAS Node, DigiDocService, pangalingid) ülevalolek (teostada vastavalt võimalustele)
- Elutukse teostus esimeses järgus võiks olla lihtne, ilma andmebaasi kasutamiseta
- Arvestada, et rakendus paigaldatakse toodangus kahes instantsis. Kummalgi instantsil erinev URL?
- Ülalkavandatud elutukseteenus on mõeldud RIA sisekasutuseks. Vajadusel piiratakse juurdepääsu ka RIA sees (IP vm meetodiga)

Elutuksepäringu vastuse näide:

```json
{
  "status": "OK",
  "started": "2018-06-10 10:08:02",
  "uptime: "P0Y0M3DT8H24M07S",
  "serviceRequests": {
    "currentDay": 10500,
    "lastDay": 670000,
    "lastWeek": 100333000
  },
  "externalServices": {
    "eIDAS Node": "OK",
    "DigiDocService": "OK",
    "Luminor": "OK",
    "SEB": "OK"
  }
}
```

## Väline elutukseteenus

Klientrakendustele pakume elutukseteenust eraldi URL-il (p.o seadistatav) ja lihtsustatud koosseisus:
- staatus
- väliste teenuste ülevalolek

Nt:

```json
{
  "status": "OK",
  "externalServices": {
    "eIDAS Node": "OK",
    "DigiDocService": "OK",
    "Luminor": "OK",
    "SEB": "OK"
  }
}
```
