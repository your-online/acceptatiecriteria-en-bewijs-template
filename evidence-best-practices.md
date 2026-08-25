# Best practices voor bewijs bij acceptatiecriteria

Voor mens en agent. Wie bewijs invult of beoordeelt, leest dit eerst.

## Het uitgangspunt

Bewijs dat ontstaat op het moment van handelen verslaat bewijs dat achteraf wordt samengesteld
([grctrail.com](https://grctrail.com/blog/soc2-evidence-collection/)). AWS noemt handmatige
verzameling achteraf een
[anti-patroon](https://docs.aws.amazon.com/wellarchitected/latest/devops-guidance/anti-patterns-for-continuous-auditing.html).
Praktisch: laat de controle zelf zijn bewijs wegschrijven.

## Sterkte van bewijsvormen, van sterk naar zwak

<a id="config-dump"></a>1. **Config-dump plus diff tegen een vastgelegde baseline.** Machine-leesbaar en diffbaar.
<a id="exit-code"></a>2. **Positieve en negatieve assertie met exit code.** De negatieve test bewijst dat de beperking
   werkt, niet alleen dat hij bestaat.
3. **Evidence bundle per criterium:** commando's, stdout/stderr, exit code, tijdstip en
   omgevingsfingerprint ([arxiv.org/pdf/2605.20456](https://arxiv.org/pdf/2605.20456)).
4. **Onafhankelijk anker:** CI-run met permalink of append-only log met hash-chain; buiten de
   agent om herleidbaar.
5. **Hash of checksum** van image, lockfile of policybestand. Bewijst identiteit, niet gedrag.
<a id="terminal-opname"></a>6. **Terminal-recording ([asciinema](https://github.com/asciinema/asciinema)).** Tekstueel, klein,
   doorzoekbaar; beter dan een screenshot van een terminal.
<a id="screenshot"></a>7. **Screenshot.** Bewijst alleen de toestand op dat moment
   ([thesoc2.com](https://www.thesoc2.com/post/what-counts-as-valid-evidence-in-soc2-type-ii-audits)).
   Alleen voor criteria waar echt een grafische interface in beeld moet; eis tijdstip en een
   herkenbaar systeem in beeld.
8. **Korte schermopname (bij voorkeur gif).** Voor gedrag dat alleen in beweging te zien is. Liever
   gif dan video, want video is zwaar en deelt lastig
   ([agileway](https://agileway.substack.com/p/why-recording-videos-for-automated)).
9. **Oordeel van een agent.** Zwakker dan programmatisch bewijs, maar met de huidige modellen
   valide mits goed uitgevoerd; zie de randvoorwaarden hieronder.

De vorm waarin een agent-oordeel bewijs wordt voor wat niet programmatisch kan, agent-bevindingen,
staat uitgewerkt in [AGENTS.md](AGENTS.md#agent-bevindingen).

## Wat alleen een mens kan

De sign-off en het dragen van het restrisico, en waarneming buiten het systeem om. De rest is
tegenwoordig ook aan een agent toe te vertrouwen, inclusief het beoordelen van de criteria zelf.

## Deugen de criteria en de tests zelf?

Een criterium of test moet inhoudelijk meten wat er gemeten moet worden, niet iets dat de
coverage hoog laat lijken. Beoordeel dat zelf, of zet er een falsifier-subagent op die toetst hoe
goed de acceptatiecriteria zijn en hoe goed de tests die erbij horen. Gebruik daarvoor een van de
sterkste modellen (Opus 5, Fable 5, GPT-5.6, Sol); die hebben hier inmiddels goed oordeelsvermogen
over. Best practices hiervoor staan in
[agentic-coding-skills](https://github.com/your-online/agentic-coding-skills).

## Drie valkuilen

**Reward hacking.** Agents halen groen door de test of de verifier zelf aan te passen. Met
filtering daarop zakte de hacked resolution rate in SpecBench van 28,57% naar 0,56%
([arxiv.org/html/2605.21384v1](https://arxiv.org/html/2605.21384v1)). Leg het verificatiescript
dus buiten de schrijfrechten van de agent, en laat het bewijs de uitgevoerde commando's tonen,
niet alleen de uitkomst.

**Een agent-oordeel is valide bewijs, maar beslist niet.** Drie randvoorwaarden. Eén: een actueel
flagship-model; het vaak aangehaalde false-positive-onderzoek is gemeten op het inmiddels ruim
voorbijgestreefde GPT-4o ([arxiv.org/pdf/2507.08794](https://arxiv.org/pdf/2507.08794)). Twee:
verse context zonder belang bij de uitkomst, want een agent die net zelf het bewijs schreef
bevestigt zijn eigen werk
([braintrust.dev](https://www.braintrust.dev/articles/what-is-llm-as-a-judge)). Drie: elke claim
verwijst naar iets aanwijsbaars. En de uitkomst blijft geen eindoordeel. Bij een beslissing over
toegang of beveiliging tekent een mens; dat gaat over verantwoordelijkheid, niet over capaciteit,
dus het blijft gelden hoe goed de modellen ook worden.

**Claimbare criteria.** Een criterium deugt pas als een agent geen "geslaagd" kan produceren
zonder dat het gedrag echt bestaat. Dat is strenger dan alleen testbaar zijn.

## Vijf aanbevelingen uit recent onderzoek

1. **Mutation-score voor testkwaliteit.** Coverage meet executie, geen verificatie;
   mutation-testing meet of tests echt falen bij foute code. Draai het op de diff
   ([Trail of Bits](https://blog.trailofbits.com/2026/04/01/mutation-testing-for-the-agentic-era/)).
2. **Onafhankelijke her-run.** Een groene run telt pas als een verse agent hem reproduceert vanaf
   een schone checkout van het commit-SHA.
3. **Bewijs buiten schrijfbereik van de bouwer.** De verifier of een CI-hook schrijft het
   bewijsrecord; alarmeer op wijzigingen aan test- en baselinebestanden in dezelfde change.
4. **Playwright-trace als eersteklas UI-bewijs.** Beantwoordt "waarom", niet alleen "wat", en is
   lastig te vervalsen. Archiveer hem bij de oplevering, want CI-retentie is 30 tot 90 dagen
   ([TestCollab](https://testcollab.com/blog/playwright-testing-evidence-at-scale)).
5. **Citatie-plicht voor agent-oordelen.** Elke claim koppelt aan een citeerbaar artefact. Bij
   beeldbeoordeling stemmen meerdere onafhankelijke beoordelingen over meerdere screenshots.

## Eén sterk bewijs boven drie zwakke

Bewijsverzameling kan zelf theater worden. Beperk het aantal bewijsvormen per criterium tot één
sterke.
