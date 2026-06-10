# Firmaradar for Salesforce

Kall **Firmaradars berikede norske selskaps-intelligens** direkte fra Salesforce Flow —
risikoscore, FIV-status (foretak i vanskeligheter), regnskapstall, eierskap,
kunngjøringer og sanksjons-/PEP-screening — og map resultatet til dine egne
Salesforce-felt. Fra ett organisasjonsnummer, uten å bygge norske
registerintegrasjoner selv.

Connectoren bruker Salesforces **External Services**-modell: du importerer Firmaradars
OpenAPI-definisjon, og hver API-operasjon blir en *invocable action* du bruker i Flow
Builder. Du velger selv hvilke operasjoner og felt du bruker — det finnes ingen
Firmaradar-side feltvelger.

> **BETA — bygget for fremtidig kundebehov.** Vi har laget connectoren slik at kunder
> som ønsker Firmaradar-intelligens i Salesforce kan komme i gang i dag. Vi har foreløpig
> ikke en egen Salesforce-org til å teste den ende-til-ende, så vi tar **veldig gjerne
> imot tilbakemeldinger** fra deg som tar den i bruk — fortell oss hva som funker og hva
> som kan bli bedre ([åpne et issue](https://github.com/Tiwas/firmaradar-salesforce/issues)
> eller kontakt oss via [firmaradar.no](https://firmaradar.no)), så samarbeider vi om
> best mulig funksjon.
>
> Gratis å installere; krever et Firmaradar-API-abonnement (din egen API-nøkkel).

---

## Installer

### Alternativ A — Deploy to Salesforce (anbefalt)

[![Deploy to Salesforce](https://raw.githubusercontent.com/afawcett/githubsfdeploy/master/deploy.png)](https://githubsfdeploy.herokuapp.com/?owner=Tiwas&repo=firmaradar-salesforce&ref=main)

Deployer Named Credential + External Service til org-en din.

### Alternativ B — `sf project deploy`

```bash
git clone https://github.com/Tiwas/firmaradar-salesforce.git
cd firmaradar-salesforce
sf project deploy start --manifest manifest/package.xml
```

### Alternativ C — importer OpenAPI-definisjonen manuelt

*Setup → External Services → Add* og lim inn / pek til:

```
https://raw.githubusercontent.com/Tiwas/firmaradar-salesforce/main/firmaradar.swagger.json
```

---

## Sett opp og bruk

1. **Legg inn API-nøkkelen din** på en External Credential / Named Principal
   (`Authorization: Bearer <din-nøkkel>`). Nøkkelen lagres i Salesforce — aldri i Flow,
   Apex eller metadata. Du finner/oppretter nøkkelen på
   [firmaradar.no](https://firmaradar.no) under *Integrasjoner → API-nøkler*.
2. **Bygg en Flow:** dra inn de genererte Firmaradar-actionene (f.eks. «Hent selskap»,
   «Risikoscore», «AML-sjekk»), send organisasjonsnummeret inn, og map svaret til
   Account-feltene dine.

Connectoren bruker ditt eksisterende Firmaradar-API-abonnement og dine aktive
utvidelser — operasjoner du ikke har tilgang til returnerer en tydelig tilgangsfeil
(aldri stille «lav risiko» eller falsk AML-status).

---

## Hva ligger i repoet

| Fil | Hva |
|-----|-----|
| `firmaradar.swagger.json` | OpenAPI 2.0-definisjonen (External Service-import) |
| `force-app/` | Named Credential + External Service-metadata (deploybar kilde) |
| `manifest/package.xml` | Deploy-manifest for `sf project deploy` |
| `sfdx-project.json` | Salesforce DX-prosjektoppsett |

## Lisens

Apache-2.0. Selve Firmaradar-tjenesten og -dataene leveres under egne vilkår — se
[firmaradar.no](https://firmaradar.no).
