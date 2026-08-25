# Acceptatiecriteria en bewijs

Een invulbaar HTML-document waarin per acceptatiecriterium wordt vastgelegd welk bewijs er is
geleverd, en waarin dat bewijs vervolgens wordt beoordeeld. Je gebruikt het wanneer een criterium
niet alleen afgevinkt moet worden, maar aantoonbaar moet zijn: iemand anders moet later kunnen
nalopen waaróp een groen vinkje gebaseerd is.

De criteria in `template.html` zijn een voorbeeldset (verificatie van een afgeschermde
ontwikkelomgeving). Vervang ze door je eigen criteria; de structuur eromheen is het template.

## Soorten bewijs

- **Script uitvoer** — Een script voert de controle uit; de letterlijke uitvoer komt hier te staan.
  Dit heeft de voorkeur, want er komt geen oordeel aan te pas.
- **Bewijsbestanden** — Alles wat niet als tekst te vangen is: screenshot, GIF, schermopname,
  logbestand. Kan de agent het niet zelf vastleggen, dan levert een persoon het aan.

## Soorten oordelen

- **Falsifier oordeel** — Een aparte beoordelaar (agent) met verse context probeert het bewijs
  onderuit te halen. Uitkomst is VALIDE of WEERLEGD.
- **Menselijk oordeel** — De eindbeoordeling door een mens. Zet de status van de check op voldaan,
  werk nodig of niet voldaan.

## Voorbeeld van een ingevulde check

> **De testsuite trekt de juiste conclusie uit een gegeven uitvoer** — Voldaan
>
> Voer twaalf bekend-foute configuraties aan de bewaker. Alle twaalf worden afgekeurd.

**Script uitvoer**

```
== zelftest van de configbewaker ==
  OK  ongewijzigde payload                    -> exit 0
  OK  wslInheritsWindowsSettings weg          -> exit 1
  OK  allowUnsandboxedCommands op true        -> exit 1
  OK  beschermd pad niet in permissions.deny  -> exit 1
  OK  _beschermd helemaal weg                 -> exit 1
  OK  /tmp niet in allowWrite                 -> exit 1
```

**Falsifier oordeel** — VALIDE. Elke foute configuratie wordt afgekeurd en de goede wordt
goedgekeurd, dus de bewaker keurt niet simpelweg alles af.

**Menselijk oordeel** — Voldaan.

## Agent-instructie

De instructie voor de agents staat in een los bestand: [`agent-instructions.md`](agent-instructions.md).
Daarin staan de prompt voor het invullen van bewijs en de prompt voor de falsifier-subagent, plus
waarom dat twee gescheiden agents met verse context zijn.

De HTML en die instructie staan los van elkaar. Je kunt het template gebruiken zonder agent — dan
vul je het met de hand in — en je kunt de instructie gebruiken zonder dit template.

## Best practices voor bewijs

Niet elk bewijs is even sterk. Een config-dump met diff of een test met exit code zegt iets anders
dan een screenshot, en een oordeel van een taalmodel is het zwakste wat er is. Welke vormen sterk
zijn, wat alleen een mens kan beoordelen, en waar het in de praktijk misgaat (reward hacking,
modeloordelen als poort, criteria die te makkelijk te claimen zijn) staat in
[`best-practices.md`](best-practices.md), met bronnen.

## Gebruik

1. Open `template.html` in een browser. Geen server, geen dependencies.
2. Vul de velden bovenaan in, pas de criteria aan, plak bewijs in de velden en sleep
   bewijsbestanden naar de dropzones.
3. Klik rechtsonder op **Bewaar als bestand**. Je krijgt een kopie van de HTML terug met alle
   ingevulde velden en bijlagen erin; die kopie is zelf weer een compleet, standalone bestand.

## Twee regels die ertoe doen

- **Bewijs is een verwijzing, geen bewering.** "Getest en werkt" is geen bewijs; uitvoer of een
  bestand wel, want een ander kan die nalopen.
- **Wie het bewijs invult, beoordeelt het niet.** Anders keurt de invuller zijn eigen werk goed en
  zegt een groen vinkje niets.
