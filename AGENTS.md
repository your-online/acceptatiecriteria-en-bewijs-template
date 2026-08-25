# Agent-instructie: bewijs invullen en controleren

Hoort bij `VERIFICATION.html`, maar werkt ook los ervan. Een ingevuld template heet een
**run-document**. Per criterium staan daarin drie velden: bewijs, falsifier-agent oordeel en
menselijk oordeel. Lees eerst
[`evidence-best-practices.md`](references/evidence-best-practices.md).

Invullen en controleren doen twee losse agents, allebei met verse context. Een agent die net zelf
bewijs invulde, is geneigd zijn eigen werk te bevestigen.

## 1. Bewijs invullen

Waar de controle een script is, schrijft het script de uitvoer zelf weg; daar komt geen agent aan
te pas en dat heeft de voorkeur. Alleen voor complexer bewijs zet je een invulagent in, met verse
context.

Prompt voor de invulagent:

```
Vul het run-document in. Per criterium:

- Voer de controle uit die onder het criterium staat.
- Plak de letterlijke script uitvoer in het veld. Verkort of herschrijf hem niet.
- Laat het script als eerste regels hostname, OS-versie, gebruiker en het unieke
  controlegetal van de build (de image-digest) printen. Daarmee is achteraf
  aantoonbaar op welke machine de run draaide.
- Kun je iets niet zelf vastleggen, laat het veld leeg en noteer dat een persoon
  dat bestand moet aanleveren.
- Lukt een controle niet, laat het veld leeg en noteer waarom.
- Bewijs is een verwijzing, geen bewering: "getest en werkt" is geen bewijs,
  uitvoer of een bestand wel.
- Vul nooit een aanname in, en beoordeel je eigen invulling niet.
```

<a id="agent-bevindingen"></a>**Agent-bevindingen.** Is een criterium niet programmatisch aan te
tonen, dan levert de invulagent bevindingen. Kijk objectief naar code, gedrag of beelden en
rapporteer wat je wel en niet vindt; "geen bewijs gevonden" is een geldig resultaat. Elke claim
verwijst naar iets aanwijsbaars: een bestandsregel, een logregel, een screenshot. Daarna probeert
een falsifier-subagent de bevindingen te weerleggen. Dat is een aparte stap, los van sectie 2, met
als hypothese "de claims kloppen".

## 2. Bewijs controleren via falsifier subagent

Aparte agent op het hoogst beschikbare flagship-model (effort-stand maakt niet uit), met verse
context, draaiend op de machine waar de controles liepen zodat hij de implementatie kan inzien.
Geef hem per criterium deze prompt en plak het antwoord in het veld Falsifier-agent oordeel:

```
Hieronder staan een acceptatiecriterium en het bewijs dat ervoor is aangeleverd:
script uitvoer, bestanden, of allebei. Jouw enige doel is dit bewijs te
weerleggen. Niet "klopt dit?" maar "laat zien dat het niet klopt".

Toets hard:
- Als de claim onwaar was, zou dit bewijs er dan nog precies zo uitzien? Een
  groene run zonder output, "getest en werkt" zonder run, een check die niet
  rood kán worden: dat overleeft een valse claim en bewijst dus niets.
- Wie produceerde het bewijs? Tekst van de agent die de claim doet is een
  verwijzing naar bewijs, nooit het bewijs zelf.
- Hoort het bewijs bij exact deze versie, of kan het ouder zijn?
- Is er een niet-geteste weg naar hetzelfde resultaat, en zijn de niet-gecheckte
  delen benoemd, in plaats van stilte die als dekking leest?
- Zijn tests gewijzigd, versmald of verwijderd? Dat is een bevinding totdat
  iemand het tegendeel toont.

Je mag de implementatie inzien. Wees niet perfectionistisch: kun je het bewijs
niet of nauwelijks weerleggen, dan houdt het stand en zeg je dat.
Zet je antwoord in het veld Falsifier-agent oordeel van deze check in het
run-document: VALIDE of WEERLEGD, plus je redenering in maximaal drie zinnen.
```

De toetsvragen komen uit criteria C6 en C7 van de
[agentic-coding-skills-rubric](https://github.com/your-online/agentic-coding-skills/blob/main/skills/references/rubric.md).

## 3. Menselijk oordeel

Een mens zet daarna de status: voldaan, werk nodig of niet voldaan. Wie het bewijs invulde,
beoordeelt niet.
