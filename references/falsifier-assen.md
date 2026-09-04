# De drie falsifier-assen

Lees dit voordat je een falsifier-oordeel schrijft of beoordeelt. `AGENTS.md` sectie 2 geeft de
korte versie en stuurt de agents weg; dit bestand is de rubriek waar elke agent uit werkt.

Vastgesteld 2 september 2026. Bronnen: de
[agentic-coding-rubric](https://github.com/your-online/agentic-coding-skills/blob/main/skills/references/rubric.md)
(C3, C5, C6, C9) en [`evidence-best-practices.md`](evidence-best-practices.md) hiernaast. Waar dit
strenger is dan de rubric staat dat erbij.

**Waarom drie oordelen en niet één.** Eén kolom slaat drie verschillende soorten falen plat tot één
woord. Het geval dat de splitsing afdwong: een rij droeg één rood `REFUTED` dat las als "de
sortering klopt niet", terwijl de weerlegging over de hashwacht ging — een eigenschap van het
*bewijs*, niet van het gedrag. Een lezer kan niets met een oordeel dat niet zegt welke van de drie
brak.

## De schaal

`VALID` · `WEAK` · `REFUTED`, per as.

`evidence-best-practices.md` beveelt een binair oordeel aan: minder spreiding tussen agents, geen
ruimte om iets half-goed te noemen om een harde keuze te ontwijken. Dit wijkt daarvan af, omdat er
een geval is dat geen van beide woorden vangt: een toets die het best haalbare is en tóch niet
waterdicht — de structurele sorteertest is het voorbeeld. De prijs is dat `WEAK` een plek wordt om
je te verstoppen.

Twee regels houden die prijs betaalbaar:

1. **`WEAK` geldt alleen met een expliciete zin** die zegt wat precies ontbreekt en wat het zou
   kosten om er `VALID` van te maken. Zonder die zin telt het oordeel niet en gaat de as terug naar
   de falsifier.
2. **`VALID` is een normale uitkomst, geen gebrek aan durf.** Een as waar je geen concreet,
   plausibel gebrek kunt benoemen is `VALID` — zeg dat gewoon en ga door. Een falsifier die op elke
   as `WEAK` teruggeeft, is opgehouden met onderscheiden: als elk criterium en elke test even
   half-goed is, leert de lezer niets en draagt de kolom geen informatie meer. Grijp naar `WEAK`
   als je een specifiek gat hebt en het kunt beprijzen; grijp naar `VALID` als de aanval die je
   werkelijk hebt geprobeerd niet landde. "Ik kan me een sterkere toets voorstellen" is geen gat —
   elke toets heeft een sterkere versie, en met die redenering is `VALID` per constructie
   onbereikbaar.

## As 1 — het criterium. Is dit de juiste eis?

1. **Het gaat over gedrag van de feature**, niet over het harnas of de meetmethode. Werkbare toets
   uit de rubric (C3): kun je dit criterium vervangen door een waarneembare uitkomst zonder dat er
   iets wegvalt? Zo ja, dan schrijft het machinerie voor waar een uitkomst zou volstaan.
2. **Het is herleidbaar naar een bron.** De featuretekst, bevroren broncode met bestand en
   regelnummer, of een genummerd besluit — waarbij een besluit ook een vastgelegde vraag-en-antwoord
   met een collega mag zijn, mits genoteerd en gerefereerd. *Strenger dan de rubric*, die alleen
   herleidbaarheid naar bron, persoon en datum vraagt; een nummer is hier vereist omdat criteria en
   vragen naar die nummers verwijzen.
3. **Falsifieerbaar, aangetoond met een aanval.** Niet "er is een waarneming denkbaar die het
   onwaar maakt", maar: bouw een plausibele implementatie die het criterium letterlijk haalt en
   toch fout, onveilig of nutteloos is. Wijs de formulering aan die dat toelaat en geef de kleinste
   aanscherping. Kun je er geen construeren, dan is het criterium gesloten — dat is een `VALID` en
   geen reden om door te graven.
4. **Niet trivialiseerbaar.** Het kan niet gehaald worden terwijl de feature stuk is.
5. **De verzameling dekt het normale, het lastige en het falende pad.** Dit oordeel gaat over de
   collectie, niet over de losse rij; rapporteer het één keer, apart.
6. **Non-goals tellen mee.** Wat expliciet buiten scope valt is criteriamateriaal, geen voetnoot.
7. **Een gat is een vraag, geen aanname.** Ontbrekende criteria leg je voor aan de eigenaar van de
   keuze; je vult ze niet zelf in.

Scopediscipline: een criterium is geschreven voor een benoemd pad en een benoemde methode. Wijzen
op het bestaan van een *ander* pad is alleen een bevinding als dat pad hetzelfde gedrag bereikt en
niemand het benoemd heeft. Anders hoort het op de non-goals-kaart, niet in een `WEAK`.

## As 2 — de test. Deugt de toets?

1. **Hij kan aantoonbaar rood worden**, en de mutatie gooit *deze* test om en niet een andere.
2. **De assertie past bij wat het criterium belooft.** Gedragscriterium → assertie op de uitkomst.
   Structuurcriterium (vijf velden in het antwoord, de tijd uit de database) → een assertie op de
   vorm is dan juist de goede en geen zwakte. `WEAK` is voor een gedragscriterium waarvan de test
   alleen bij de vorm kan komen.
   *Aanvulling uit de rubric (C5):* toets eerst of het structuurcriterium zélf als gedragscriterium
   geformuleerd had kunnen worden. Zo ja, dan zit het gebrek in het criterium en niet in de test, en
   luidt het oordeel `WEAK` **met de gedragsformulering erbij** — nooit `REFUTED`. Een criterium
   herschrijven is de beslissing van de eigenaar, niet van de falsifier.
3. **Grenswaarden zijn gedekt**, niet alleen het gelukkige pad.
4. **Positieve en negatieve assertie.** Een negatieve assertie laat zien dat een beperking *werkt*,
   niet alleen dat hij bestaat; "geweigerd" zonder een bijbehorend "lukt wel"-geval is niet te
   onderscheiden van een kapotte meting. Een lege uitkomst is nooit een groene uitweg.
5. **De dekking is proportioneel; overdekking is ook een gebrek.** Stopregel: elke materiële test
   is te herleiden tot een criterium — normaal geval, grenswaarde of faalpad — en elk zo'n criterium
   heeft een test die rood kan worden. Een test die niets toevoegt aan wat al gedekt is, of die
   alleen de vorm van de implementatie herhaalt, is overdekking. Rapporteer die **met de naam van de
   test en een voorstel hem te schrappen**; een as die alleen naar gaten zoekt vindt altijd gaten en
   dan groeit de suite eindeloos. Schrappen is de beslissing van de eigenaar, niet van de falsifier.
6. **Een gewijzigde, versmalde of verwijderde test is een bevinding** totdat is aangetoond dat de
   eis verschoven is of de test aantoonbaar fout was, dat de vervanger eerst rood is gezien, en dat
   de resterende dekking bij naam even goed of beter is.
7. **Ontbrekend geautomatiseerd bewijs mag**, mits het expliciet als gat benoemd is en herhaalbaar
   is door iemand anders dan de auteur. Stilte die als dekking leest is een bevinding.

## As 3 — het bewijs

[`evidence-best-practices.md`](evidence-best-practices.md) geldt onverkort. De kern:

1. **De toetsvraag.** Zou dit bewijs er precies zo uitzien als de claim onwaar was? Zo ja, dan
   bewijst het niets.
2. **Letterlijke uitvoer**, met een omgevingsvingerafdruk en versie-identiteit — commit-SHA of
   image-digest, een schone werkmap, en elke seedhash waar de meting van afhangt.
3. **Eén run.** Meting en mutanten dragen dezelfde seedhash.
4. **Mutantdrift.** Een mutatietoets die na een refactor stilletjes niet meer aanslaat telt als
   rood, niet als overgeslagen.
5. **Reward hacking.** Een wijziging aan test- of baselinebestanden in dezelfde verandering is een
   bevinding tot het tegendeel blijkt.
6. **Geen bewijstheater.** Meer artefacten is niet meer zekerheid; één sterke vorm verslaat drie
   zwakke.

## Beperkingen en risico's

Naast het oordeel per as rapporteert de falsifier beperkingen en risico's waar hij tegenaan loopt:
iets dat vandaag werkt maar omvalt onder een benoembare voorwaarde, een aanname die stil in de
opzet zit, een afhankelijkheid waar niemand naar kijkt.

Maar **alleen als ze echt zijn.** Geen verzonnen scenario's, geen lijst voor de volledigheid, geen
risico dat alleen bestaat omdat er van een falsifier een risico verwacht wordt. Een risico hoort
alleen in het rapport als je kunt zeggen wat het gevolg is en waarom je denkt dat het optreedt. Loop
je nergens tegenaan, dan is "geen beperkingen gevonden" het juiste antwoord en geen teken van een
zwakke ronde. Verzonnen risico's kosten meer dan ze opleveren: ze trekken de aandacht weg van de
echte en maken het dossier onleesbaar voor wie ermee aan de slag moet.

## Opzet

Drie agents, één as per agent, elk met verse context en zonder de redenering die het bewijs heeft
voortgebracht. Een agent die het bewijs net heeft goedgekeurd, is milder over de test die het
opleverde; die kruisbesmetting is de reden voor de splitsing. Drie keer de kosten, en dat is
geaccepteerd.

Goedkopere terugval bij lage inzet: één agent, drie afzonderlijke oordelen — maar ze komen uit één
hoofd en driften dezelfde kant op, dus de drie woorden hangen sterker samen dan ze lijken.
