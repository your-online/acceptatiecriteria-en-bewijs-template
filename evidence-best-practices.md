# Best practices voor bewijs bij acceptatiecriteria

Voor mens én agent. Wie bewijs invult of beoordeelt, leest dit eerst.

## Het uitgangspunt

Bewijs dat ontstaat op het moment van handelen verslaat bewijs dat achteraf wordt samengesteld
([grctrail.com](https://grctrail.com/blog/soc2-evidence-collection/); AWS noemt handmatige
verzameling achteraf een [anti-patroon](https://docs.aws.amazon.com/wellarchitected/latest/devops-guidance/anti-patterns-for-continuous-auditing.html)).
Praktisch: laat de controle zelf zijn bewijs wegschrijven.

## Sterkte van bewijsvormen, van sterk naar zwak

<a id="config-dump"></a>1. **Config-dump plus diff tegen een vastgelegde baseline.** Machine-leesbaar en diffbaar.
<a id="exit-code"></a>2. **Positieve én negatieve assertie met exit code.** De negatieve test bewijst dat de beperking
   wérkt, niet dat hij bestaat.
3. **Evidence bundle per criterium:** commando's, stdout/stderr, exit code, tijdstip en
   omgevingsfingerprint ([arxiv.org/pdf/2605.20456](https://arxiv.org/pdf/2605.20456)).
4. **Onafhankelijk anker:** CI-run met permalink of append-only log met hash-chain; buiten de
   agent om herleidbaar.
5. **Hash of checksum** van image, lockfile of policybestand. Bewijst identiteit, niet gedrag.
<a id="terminal-opname"></a>6. **Terminal-recording ([asciinema](https://github.com/asciinema/asciinema)).** Tekstueel, klein,
   doorzoekbaar; strikt beter dan een screenshot van een terminal.
<a id="screenshot"></a>7. **Screenshot.** Bewijst alleen de toestand op dat moment
   ([thesoc2.com](https://www.thesoc2.com/post/what-counts-as-valid-evidence-in-soc2-type-ii-audits)).
   Alleen voor criteria waar echt een grafische interface in beeld moet; eis tijdstip en herkenbaar
   systeem in beeld.
8. **Korte schermopname (bij voorkeur gif).** Voor gedrag dat alleen in beweging te zien is. Liever
   gif dan video: video is zwaar en deelt lastig
   ([agileway](https://agileway.substack.com/p/why-recording-videos-for-automated)).
9. **Oordeel van een agent.** Zwakker dan programmatisch bewijs, maar met de huidige modellen
   valide mits goed uitgevoerd; zie de randvoorwaarden hieronder.

**Agent-bevindingen** — de vorm waarin een agent-oordeel bewijs wordt voor wat niet programmatisch
kan — staan uitgewerkt in [AGENTS.md](AGENTS.md#agent-bevindingen).

## Wat alleen een mens kan

Akkoord dat het criterium het juiste criterium is; het oordeel over subjectieve criteria zoals
werkbaarheid; de sign-off en het dragen van het restrisico. De valkuil is dat subjectieve criteria
zich verstoppen tussen de mechanische
([paelladoc.com](https://paelladoc.com/blog/acceptance-criteria-for-ai-agents/)).

## Drie valkuilen

**Reward hacking.** Agents halen groen door de test of de verifier zelf aan te passen; met
filtering daarop zakte de hacked resolution rate in SpecBench van 28,57% naar 0,56%
([arxiv.org/html/2605.21384v1](https://arxiv.org/html/2605.21384v1)). Gevolg: het
verificatiescript ligt buiten de schrijfrechten van de agent, en het bewijs toont de uitgevoerde
commando's, niet alleen de uitkomst.

**Een agent-oordeel is valide bewijs, maar beslist niet.** Randvoorwaarden: een actueel
flagship-model (het vaak aangehaalde false-positive-onderzoek is gemeten op het inmiddels ruim
voorbijgestreefde GPT-4o, [arxiv.org/pdf/2507.08794](https://arxiv.org/pdf/2507.08794)); verse
context zonder belang bij de uitkomst (self-enhancement bias,
[braintrust.dev](https://www.braintrust.dev/articles/what-is-llm-as-a-judge)); en elke claim
verwijst naar iets aanwijsbaars. De uitkomst blijft géén eindoordeel: bij een beslissing over
toegang of beveiliging tekent een mens — een verantwoordelijkheidsargument, geen
capaciteitsargument, dus het blijft gelden hoe goed de modellen ook worden.

**Claimable criteria.** Een criterium deugt pas als een agent geen "geslaagd" kan produceren
zonder dat het gedrag echt bestaat. Dat is strenger dan alleen testbaar zijn.

## Vijf aanbevelingen uit recent onderzoek

1. **Mutation-score voor testkwaliteit.** Coverage meet executie, geen verificatie;
   mutation-testing meet of tests echt falen bij foute code. Draai het op de diff
   ([Trail of Bits](https://blog.trailofbits.com/2026/04/01/mutation-testing-for-the-agentic-era/)).
2. **Onafhankelijke her-run.** Een groene run telt pas als een verse agent hem reproduceert vanaf
   een schone checkout van het commit-SHA.
3. **Bewijs buiten schrijfbereik van de bouwer.** De verifier of een CI-hook schrijft het
   bewijsrecord; alarmeer op wijzigingen aan test- en baselinebestanden in dezelfde change.
4. **Playwright-trace als eersteklas UI-bewijs.** Beantwoordt "waarom", niet alleen "wat"; lastig
   te vervalsen. Archiveer bij de oplevering — CI-retentie is 30–90 dagen
   ([TestCollab](https://testcollab.com/blog/playwright-testing-evidence-at-scale)).
5. **Citatie-plicht voor agent-oordelen.** Elke claim koppelt aan een citeerbaar artefact; bij
   beeldbeoordeling stemmen meerdere onafhankelijke beoordelingen over meerdere screenshots.

## Eén sterk bewijs boven drie zwakke

Bewijsverzameling kan zelf theater worden. Beperk het aantal bewijsvormen per criterium tot één
sterke.
