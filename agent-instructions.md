# Agent-instructie: bewijs invullen en controleren

Lees eerst [`bewijsstandaard.md`](bewijsstandaard.md): daar staat welke bewijsvormen sterk zijn,
welke zwak, en waar het misgaat.

Deze instructie hoort bij het template `template.html`, maar staat er los van. Je kunt de
werkwijze hieronder ook gebruiken zonder dat template, en het template gebruiken zonder agent.

Het gaat om een document waarin per acceptatiecriterium bewijs wordt vastgelegd en beoordeeld.
Per criterium zijn er drie velden: het bewijs (script uitvoer of bewijsbestanden), het falsifier
oordeel, en het menselijk oordeel.

## Waarom twee losse agents met verse context

Twee losse agents, allebei met verse context. Dat is geen formaliteit: een agent die net zelf het
bewijs heeft ingevuld, is geneigd zijn eigen werk te bevestigen. Door ze te scheiden doet de eerste
alleen wat de uitvoer laat zien, en heeft de tweede geen belang bij een gunstige uitkomst.

## 1. Bewijs invullen

Waar de controle een script is, laat je het script de uitvoer zelf wegschrijven. Daar komt geen
agent aan te pas, en dat is de voorkeur: hoe minder tussenkomst, hoe minder ruimte om het bewijs te
kleuren. Alleen waar het bewijs complexer is dan een scriptuitvoer zet je er een invulagent op, met
verse context.

Prompt voor de invulagent:

```
Vul dit document in. Per criterium:

- Voer de controle uit die onder het criterium staat.
- Plak de letterlijke script uitvoer in het veld. Verkort of herschrijf hem niet.
- Kun je iets niet zelf vastleggen, bijvoorbeeld een schermafbeelding, laat het
  veld dan leeg en noteer dat een persoon dat bestand moet aanleveren.
- Lukt een controle niet, laat het veld leeg en noteer waarom.
- Bewijs is een verwijzing, geen bewering. "Getest en werkt" is geen bewijs;
  uitvoer of een bestand wel, want een ander kan die nalopen.
- Vul nooit een aanname in, en beoordeel je eigen invulling niet.
```

## 2. Bewijs controleren via falsifier subagent

Aparte agent op het hoogst beschikbare flagship-model, bijvoorbeeld Opus 5, Fable 5, GPT-5.6 of Sol (effort-stand maakt niet uit), met verse context,
draaiend op de machine waar de controles zijn uitgevoerd zodat hij de implementatie kan inzien.
Geef hem per criterium deze prompt en plak zijn antwoord in het veld Falsifier oordeel.

```
Hieronder staan een acceptatiecriterium en het bewijs dat ervoor is aangeleverd:
script uitvoer, bestanden, of allebei. Probeer te weerleggen dat dit bewijs het
criterium aantoont.

Kijk of het bewijs echt aantoont wat er beweerd wordt, of er een niet geteste weg
naar hetzelfde resultaat bestaat, of het bewijs er ook zo uit zou zien zonder de
beperking, en of het volledig is. Een bewering zonder verwijzing telt niet als
bewijs: "getest en werkt" is onvoldoende, uitvoer of een bestand niet.

Je mag de implementatie inzien. Verzin geen bezwaren om iets te moeten leveren.
Antwoord met VALIDE of WEERLEGD, plus je redenering in maximaal drie zinnen.
```

## 3. Menselijk oordeel

Een mens zet daarna de status: voldaan, werk nodig of niet voldaan. Wie het bewijs invulde
beoordeelt niet, anders keurt de invuller zijn eigen werk goed en zegt een groen vinkje niets.
