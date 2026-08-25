# Agent-instructie: bewijs invullen en controleren

Hoort bij `template.html`, maar werkt ook los ervan. Per criterium zijn er drie velden: bewijs,
falsifier-agent oordeel en menselijk oordeel. Lees eerst
[`evidence-best-practices.md`](evidence-best-practices.md).

Invullen en controleren doen **twee losse agents, allebei met verse context**: een agent die net
zelf bewijs invulde, is geneigd zijn eigen werk te bevestigen.

## 1. Bewijs invullen

Waar de controle een script is, schrijft het script de uitvoer zelf weg; daar komt geen agent aan
te pas en dat heeft de voorkeur. Alleen voor complexer bewijs zet je een invulagent in, met verse
context.

Prompt voor de invulagent:

```
Vul dit document in. Per criterium:

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
tonen, dan levert de invulagent bevindingen: kijk objectief naar code, gedrag of beelden en
rapporteer wat je wel én niet vindt — "geen bewijs gevonden" is een geldig resultaat. Elke claim
verwijst naar iets aanwijsbaars (bestandsregel, logregel, screenshot). Daarna probeert een
falsifier-subagent de bevindingen te weerleggen; dat is een aparte stap, los van sectie 2, met als
hypothese "de claims kloppen".

## 2. Bewijs controleren via falsifier subagent

Aparte agent op het hoogst beschikbare flagship-model (effort-stand maakt niet uit), verse
context, draaiend op de machine waar de controles liepen zodat hij de implementatie kan inzien.
Geef hem per criterium deze prompt en plak het antwoord in het veld Falsifier-agent oordeel:

```
Hieronder staan een acceptatiecriterium en het bewijs dat ervoor is aangeleverd:
script uitvoer, bestanden, of allebei. Probeer te weerleggen dat dit bewijs het
criterium aantoont.

Kijk of het bewijs echt aantoont wat er beweerd wordt, of er een niet geteste weg
naar hetzelfde resultaat bestaat, of het bewijs er ook zo uit zou zien zonder de
beperking, en of het volledig is. Een bewering zonder verwijzing telt niet als
bewijs.

Je mag de implementatie inzien. Verzin geen bezwaren om iets te moeten leveren.
Antwoord met VALIDE of WEERLEGD, plus je redenering in maximaal drie zinnen.
```

## 3. Menselijk oordeel

Een mens zet daarna de status: voldaan, werk nodig of niet voldaan. Wie het bewijs invulde,
beoordeelt niet.
