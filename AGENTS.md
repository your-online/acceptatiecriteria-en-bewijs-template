# Agent-instructie: bewijs invullen en controleren

Hoort bij `VERIFICATION.html`, maar werkt ook los ervan. Een ingevuld template heet een
**run-document**. Per criterium staan daarin drie stappen: bewijs, een falsifier-oordeel op drie assen
(criterium, test, bewijs) en het menselijk oordeel. Lees eerst
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

## 2. Bewijs controleren via falsifier subagents

Het oordeel valt in **drie assen**, niet in één. Eén kolom slaat drie verschillende soorten falen
plat tot één woord: het geval dat dit afdwong was een rij met één rood `REFUTED` dat las als "de
sortering klopt niet", terwijl de weerlegging over de hashwacht ging — een eigenschap van het
*bewijs*, niet van het gedrag. Een lezer kan niets met een oordeel dat niet zegt welke van de drie
brak.

| As | Vraag |
|---|---|
| 1. Criterium | Is dit de juiste eis? |
| 2. Test | Deugt de toets? |
| 3. Bewijs | Toont het bewijs de claim aan? |

Schaal per as: `VALID` · `WEAK` · `REFUTED`. **`WEAK` telt alleen mee met een zin die zegt wát
ontbreekt en wat `VALID` zou kosten** — zonder die zin gaat de as terug naar de falsifier. En
`VALID` is een normale uitkomst, geen gebrek aan durf: een falsifier die overal `WEAK` teruggeeft
is opgehouden met onderscheiden.

Drie agents, één as per agent, elk op het hoogst beschikbare flagship-model met verse context,
draaiend op de machine waar de controles liepen zodat ze de implementatie kunnen inzien. Een agent
die het bewijs net heeft goedgekeurd is milder over de test die het opleverde; die kruisbesmetting
is de reden voor de splitsing. Bij lage inzet mag het met één agent die drie afzonderlijke
oordelen velt, maar die komen uit één hoofd en driften dezelfde kant op.

De volledige rubriek per as staat in [`falsifier-assen.md`](references/falsifier-assen.md). Geef
elke agent dat bestand mee plus deze prompt, en plak het antwoord in het veld van zijn as:

```
Hieronder staan een acceptatiecriterium, de toets die eronder hangt en het
bewijs dat is aangeleverd. Jouw enige doel is as <N> te weerleggen. Niet
"klopt dit?" maar "laat zien dat het niet klopt".

Lees references/falsifier-assen.md en werk uit de rubriek van jouw as.

As 1 - het criterium. Gaat het over gedrag van de feature en niet over het
harnas? Is het herleidbaar naar een genummerde bron? Falsifieerbaar
aangetoond met een aanval: bouw een plausibele implementatie die het
criterium letterlijk haalt en toch fout of nutteloos is, wijs de formulering
aan die dat toelaat, en geef de kleinste aanscherping.

As 2 - de test. Kan hij aantoonbaar rood worden, en gooit de mutatie deze
test om en niet een andere? Past de assertie bij wat het criterium belooft?
Grenswaarden, positief en negatief? En de andere kant op: is er overdekking,
een test die niets toevoegt aan wat al gedekt is? Noem die bij naam met een
voorstel hem te schrappen.

As 3 - het bewijs. Zou dit bewijs er precies zo uitzien als de claim onwaar
was? Letterlijke uitvoer met omgevingsvingerafdruk en versie-identiteit? Eén
run? Zijn test- of baselinebestanden in dezelfde verandering aangeraakt?

Je mag de implementatie inzien. Wees niet perfectionistisch: landt de aanval
die je werkelijk probeerde niet, dan is dat VALID en zeg je dat.

Antwoord met VALID, WEAK of REFUTED plus je redenering in maximaal drie
zinnen. Bij WEAK is een zin verplicht die zegt wát ontbreekt en wat VALID zou
kosten. Rapporteer daarnaast alleen beperkingen en risico's die echt zijn -
"geen beperkingen gevonden" is een geldig antwoord.
```

De toetsvragen van as 3 komen uit criteria C6 en C7 van de
[agentic-coding-skills-rubric](https://github.com/your-online/agentic-coding-skills/blob/main/skills/references/rubric.md);
as 1 en 2 leunen op C3, C5 en C9.

**Reactie van de uitvoerder.** Onder een oordeel mag de uitvoerder een reactie zetten: wat hij met
de aanval heeft gedaan. Die reactie verandert het oordeel niet — anders beoordeelt de maker van het
bewijs alsnog zijn eigen werk. Hij lost wel op dat een lezer moet raden of een `WEAK` is blijven
liggen of juist aanleiding was om iets te veranderen. De as blijft `WEAK` tot een falsifier de
nieuwe formulering heeft aangevallen.

## 3. Menselijk oordeel

Een mens zet daarna de status: voldaan, werk nodig of niet voldaan. Wie het bewijs invulde,
beoordeelt niet.
