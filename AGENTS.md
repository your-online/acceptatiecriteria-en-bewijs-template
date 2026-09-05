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

## De meetgrens: oordeel op de hunks, niet op de bestanden

Een conformiteitstoets beantwoordt de vraag of dit werk de regels volgt, niet of de repository schoon
is. Die twee lopen meteen uiteen: raak één regel aan in een bestand van vierhonderd regels, en een
toets die per bestand oordeelt geeft je alles terug wat dat bestand sinds zijn geboorte heeft
verzameld. De rij staat dan rood om werk dat niemand in deze oplevering gedaan heeft, en de
beoordelaar kan jouw gebreken niet meer onderscheiden van de geërfde. Het is ook de snelste manier om
te zorgen dat niemand het document nog leest.

Begrens dus de **bevinding**, niet het lezen. Lees het hele bestand — dat heb je nodig, want vrijwel
niets wat de moeite waard is staat in één losse regel: of een methode async is, of `base.Configure()`
het eerste statement is, of een filter op een al opgehaalde lijst zit. Meld daarna alleen wat op een
regel binnen een gewijzigde hunk valt. Een toets die letterlijk alleen de difftekst als invoer krijgt
is een slechtere toets, geen strengere: hij mist juist de context die hem correct maakt.

Drie soorten regels breken onder deze begrenzing, en elk daarvan moet de toets met zoveel woorden
benoemen. Doe je dat niet, dan slikt de grens stilletjes echte gebreken in — en dat is gevaarlijker
dan de ruis die hij moest wegnemen.

- **Bestaansregels** — een nieuw endpoint hoort een requestbestand te hebben, een testbestand een
  onderwerp dat bestaat. Er is geen hunk om aan te haken als het punt juist is dat er iets ontbreekt.
  Grens: het hele bestand, en alleen als dat bestand nieuw is in deze oplevering.
- **Verwijderregels** — vervangen code is weg, geen verweesde tests, geen dode verwijzingen. Het
  bewijs zit in de *verwijderde* regels. Een toets die alleen toegevoegde regels leest, ziet het
  nooit; hij moet ook de verwijderde hunks lezen.
- **Repo-brede invarianten** — één registratie per interface, geen dubbele route, geen tweede type
  met dezelfde naam. Die zijn alleen over de hele boom zinvol. Meet repo-breed, maar meld alleen als
  een gewijzigde regel deel uitmaakt van wat de invariant breekt; anders rapporteer je de botsing van
  een ander.

En betaal de prijs die de grens maakt, anders wordt hij een manier om er goed uit te zien. Alles wat
de toets buiten de hunks zag en gemeld zou hebben, gaat naar een **bevindingenlijst voor de eigenaar**
— genoemd, geteld, in het document, maar buiten de criteriumrijen. Zonder die lijst leest het document
alsof de repository schoon is, terwijl de toets net heeft aangetoond van niet. Een grens die stilletjes
weggooit wat hij zag, is geen grens maar een filter op slecht nieuws.

### Oordelen landen in het document, altijd

Een oordeel dat in een chatbericht, in het eindverslag van een agent of in een JSON-bestand op
schijf blijft staan, is niet opgeleverd. Het rundocument is de oplevering; al het andere is
werkmateriaal. De laatste stap van een falsifierronde is dus nooit "terugrapporteren", maar dat
elke as van elk criterium een oordeel draagt of een streep. Niemand hoort daarom te hoeven vragen,
en een ronde is niet af zolang het er niet staat.

**De falsifier schrijft het document niet; de uitvoerder doet dat.** Falsifiers draaien met verse
context en naast elkaar — daarvoor worden ze juist per as en per sectie gesplitst — en agents die
tegelijk in één bestand schrijven, overschrijven elkaar. Geef elke falsifier één uitvoerbestand,
genoemd naar zijn as en zijn sectie, met per criterium één regel. Voeg ze daarna samen, en laat de
generator die map bij elke run inlezen, zodat samenvoegen geen stap is die iemand kan vergeten. Een
oordeel dat wacht op een mens die aan een koppelstap denkt, blijft een dag in een bestand liggen.

**Voeg samen zodra er een terugkomt, niet aan het eind.** Alles tegelijk inbouwen voelt netter en
kost de lezer een dag zicht. Een half gevuld document is eerlijker dan een leeg document met een
belofte.

Drie regels voor het samenvoegen zelf:

- **Letterlijk overnemen.** Verzacht, vat niet samen en rijm niets glad tijdens het overzetten.
  Heeft een falsifier iets weerlegd dat jij gebouwd hebt, dan gaat de weerlegging er heel in. De
  neiging om te snoeien is het sterkst op precies de plek waar snoeien de meeste schade doet.
- **Wat er al staat wint.** Een oordeel dat al in het document staat kan een met de hand toebedeelde
  as dragen, of een reactie van de uitvoerder op een eerdere aanval; een nieuwe ronde vult aan en
  overschrijft niet.
- **Geen oordeel is een streep, nooit een gunstig vermoeden.** Een as die niemand heeft aangevallen,
  is een as die niemand heeft aangevallen, en dat hoort de rij te zeggen.

Twee assen hebben helemaal geen bewijs nodig. De criterium-as vraagt of dit de juiste eis is — de
tekst volstaat. De test-as vraagt of de toets rood kan worden — de toets zelf volstaat. Alleen de
bewijs-as wacht op een run. Start die twee dus zodra de criteria en de toetsen bestaan, in plaats
van de hele ronde op te houden tot het bewijs vastligt; wachten met aanvallen tot er bewijs is, is
de gebruikelijkste manier waarop een ronde er nooit komt.

### Proportionaliteit: welke aanvallen tellen

Een falsifier die in drie van de vier assen iets vindt, vindt niets bijzonders meer. De lat is dan
verschoven van *is dit de juiste eis* naar *is dit de best denkbare eis*, en dat zijn twee
verschillende vragen — op de tweede is altijd een antwoord te bedenken.

**Een aanval telt als een bekwame collega hem plausibel zou maken**: per ongeluk, uit gemak, of in
navolging van een precedent dat vlakbij in deze codebase al bestaat. `Skip=` op zes falende tests
zetten om een groene suite te krijgen is gemak, en dat telt. Eén betekenisloze logregel per tak
schrijven om een loggingregel te halen is opzettelijk gaming, en dat telt niet. De vorm van de
dichtstbijzijnde bestaande handler overnemen telt wél, ook als die vorm fout is — juist dan, want zo
wordt de fout in werkelijkheid gemaakt.

**De uitzondering is alles over beveiliging, toegang en rechten.** Daar is een tegenstander juist het
model, dus een aanval die opzet vereist telt daar wel. Rek die uitzondering niet op naar conventies
en hygiëne door ze beveiligingsachtig te noemen.

**VALIDE is de normale uitkomst voor een goed geformuleerd criterium.** Komt minder dan ongeveer de
helft van een set valide terug, kijk dan nog een keer voordat je rapporteert: je toetst dan
waarschijnlijk aan een ideaal en niet aan de eis. Noem je eigen verdeling in één regel aan het eind.
Dat getal is zelf bewijs over je kalibratie, en het stelt de lezer in staat je te wegen.

**Val een rij niet aan op wat een naburige rij al dekt.** Overlap is een eigenschap van de set, geen
gebrek in één criterium. Noem het één keer als opmerking over de set en laat de rij met rust.

**"Spreekt zijn bereik niet uit" is op zichzelf geen zwakte.** Een criterium dat een bekwame lezer
goed zou toepassen, doet zijn werk; eisen worden gelezen door mensen en niet ontleed door een
machine. Het wordt pas een zwakte als je twee lezingen kunt noemen die tot *ander werk* leiden —
noem ze dan allebei.

### Drie vormen die weerlegd blijven, wat proportionaliteit ook zegt

Proportionaliteit snoeit speculatieve aanvallen. Ze verzacht deze drie niet. Een set die met nul
weerleggingen terugkomt is even verdacht als een set die vrijwel geheel weerlegd is, want meestal
betekent het dat juist deze drie als zwakte zijn weggeschreven.

- **Je haalt hem door iets weg te laten.** Falende tests overslaan, de rijen die tegenspreken
  verwijderen, de gedocumenteerde verzameling versmallen tot hij klopt. Is de goedkoopste weg naar
  groen iets weghalen, dan meet het criterium aanwezigheid en geen inhoud.
- **Hij bindt aan een bron die dat niet zegt.** Een criterium dat een instructiebestand, een besluit
  of een norm aanhaalt die op die plek de eis niet draagt, is nergens aan verankerd. Dat de regel op
  zichzelf verstandig kan zijn doet er niet toe: de rij claimt een mandaat dat hij niet heeft. Noem
  erbij wat de bron wel zegt.
- **De toets kan niet rood worden.** Een vastpintest op een schemafeit, een assertie op een waarde
  die per constructie vastligt, een vergelijking die blijft zwijgen als je hem door onzin vervangt.
  Dat is geen zwakke toets; dat is geen toets.

Al het andere -- een ontbrekend randgeval, een onuitgesproken aanname, een aanval die opzet vereist
-- is hoogstens een zwakte, en vaak niets.

Niets hiervan vraagt je om mild te zijn. De bevindingen die ertoe doen — een toets die niet rood kan
worden, een criterium dat je haalt door iets weg te laten, bewijs dat bij een andere versie hoort —
zijn meer waard wanneer ze niet bedolven liggen onder een dozijn opmerkingen die op elke eis ooit
geschreven van toepassing zouden zijn.

## 3. Menselijk oordeel

Een mens zet daarna de status: voldaan, werk nodig of niet voldaan. Wie het bewijs invulde,
beoordeelt niet.
