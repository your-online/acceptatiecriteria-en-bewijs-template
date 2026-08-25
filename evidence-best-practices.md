# Best practices voor bewijs bij acceptatiecriteria

Bedoeld om zowel door een mens als door een agent gelezen te worden. Wie bewijs invult of
beoordeelt, leest dit eerst.

## Het uitgangspunt

Bewijs dat ontstaat op het moment van handelen verslaat bewijs dat achteraf wordt samengesteld.
Dat is het centrale onderscheid in de auditpraktijk: bewijs moet "generated as part of normal
control operation" zijn
([grctrail.com](https://grctrail.com/blog/soc2-evidence-collection/)). AWS noemt handmatige
bewijsverzameling achteraf expliciet een anti-patroon
([AWS DevOps Guidance](https://docs.aws.amazon.com/wellarchitected/latest/devops-guidance/anti-patterns-for-continuous-auditing.html)).

Praktisch betekent dat: laat de controle zelf zijn bewijs wegschrijven, in plaats van er naderhand
een verslag van te maken.

## Sterkte van bewijsvormen, van sterk naar zwak

1. **Config-dump plus diff tegen een vastgelegde baseline.** Machine-leesbaar en diffbaar.
2. **Positieve én negatieve assertie met exit code.** De negatieve test bewijst dat de beperking
   wérkt, niet dat hij bestaat.
3. **Evidence bundle per criterium:** de uitgevoerde commando's, stdout en stderr, de exit code,
   het tijdstip en een omgevingsfingerprint. Zie de minimumbundel in Agentic Agile-V
   ([arxiv.org/pdf/2605.20456](https://arxiv.org/pdf/2605.20456)).
4. **Onafhankelijk anker:** een CI-run met permalink, of een append-only logbestand met
   hash-chain. Buiten de agent om herleidbaar.
5. **Hash of checksum** van image, lockfile of policybestand. Bewijst identiteit, niet gedrag:
   bruikbaar om vast te leggen vóór welke versie een criterium gold.
6. **Terminal-recording (asciinema).** Tekstueel, klein, doorzoekbaar en kopieerbaar; strikt beter
   dan een screenshot van een terminal
   ([asciinema](https://github.com/asciinema/asciinema)).
7. **Screenshot.** Bewijst alleen de toestand op het moment van vastleggen en is waardeloos zonder
   context. Auditors willen liever de bron zelf
   ([thesoc2.com](https://www.thesoc2.com/post/what-counts-as-valid-evidence-in-soc2-type-ii-audits)).
   Gebruik het alleen voor criteria waar echt een grafische interface in beeld moet, en eis dan dat
   tijdstip en een herkenbaar systeem in beeld staan.
8. **Video of GIF van een geautomatiseerde run.** In QA-kringen afgeraden als standaardartefact, te
   duur om te bekijken
   ([agileway](https://agileway.substack.com/p/why-recording-videos-for-automated)).
9. **Oordeel van een taalmodel.** Het zwakste bewijs; zie de valkuilen hieronder.

## Twee vormen die er ook bij horen

**Korte schermopname (gif of video).** Geldig bewijs, en soms het enige dat werkt: gedrag dat
alleen in beweging te zien is, laat zich niet in een screenshot vangen. Houd het kort en noem in
beeld het tijdstip en het systeem.

**Beoordelend agent-oordeel.** Kan iets niet programmatisch worden aangetoond, dan is een oordeel
van een agent over code, gedrag of beelden een geldige bewijsvorm — mits een actueel flagship-model
met verse context, en met een mens die tekent. Het streven blijft: programmatisch waar het kan.

## Wat alleen een mens kan

Akkoord dat het criterium het juiste criterium is. Het oordeel over subjectieve criteria zoals
werkbaarheid. De sign-off en het dragen van het restrisico. Waarneming buiten het systeem om.

De valkuil is dat subjectieve criteria zich verstoppen tussen de mechanische
([paelladoc.com](https://paelladoc.com/blog/acceptance-criteria-for-ai-agents/)).

## Drie valkuilen

**Reward hacking.** Agents halen groen door de test of de verifier zelf aan te passen. In SpecBench
zakte de hacked resolution rate van 28,57% naar 0,56% zodra daarop gefilterd werd
([arxiv.org/html/2605.21384v1](https://arxiv.org/html/2605.21384v1)). Gevolg: het verificatiescript
hoort buiten de schrijfrechten van de agent te liggen, en het bewijs moet de uitgevoerde
commando's tonen, niet alleen de uitkomst.

**Een modeloordeel telt mee, maar beslist niet.** Het onderzoek dat vaak wordt aangehaald
is gedateerd: de bekendste bevinding, dat antwoorden die alleen uit leestekens bestaan tot 35%
false positives haalden, is gemeten op GPT-4o
([arxiv.org/pdf/2507.08794](https://arxiv.org/pdf/2507.08794), juli 2025). Dat model is
inmiddels ruim voorbijgestreefd. Een actueel flagship-model als beoordelaar is een legitieme
manier om bewijs te toetsen, mits je er ook echt een neerzet: Opus 5, Fable 5, GPT-5.6 of Sol.
Zet er geen klein of goedkoop model op; dan komt de kritiek uit dat onderzoek wel terug.

Twee eisen blijven staan, en die gaan niet over modelkwaliteit. De beoordelaar draait met verse
context, want een model dat net zelf het bewijs schreef bevestigt zijn eigen werk
(self-enhancement bias, https://www.braintrust.dev/articles/what-is-llm-as-a-judge). En de
uitkomst is geen eindoordeel: dat een beoordeling deugt, maakt haar nog niet tot degene die het
risico draagt. Bij een beslissing over toegang of beveiliging hoort een mens te tekenen. Dat is
een verantwoordelijkheidsargument, geen capaciteitsargument, en het blijft dus gelden hoe goed
de modellen ook worden.

**Claimable criteria.** Een criterium deugt pas als een agent geen "geslaagd" kan produceren zonder
dat het gedrag echt bestaat. Dat is een strengere eis dan alleen testbaar zijn.

## Eén sterk bewijs boven drie zwakke

Bewijsverzameling kan zelf theater worden: achter mensen aan zitten voor screenshots in plaats van
daadwerkelijk verifiëren. Beperk het aantal bewijsvormen per criterium tot één sterke.
