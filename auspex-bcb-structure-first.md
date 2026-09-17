# AUSPEX·BCB — STRUCTURE FIRST
## Which axes to read Copom communications on, and what functions and structures the reading should be built from

*Companion to the Axes of Text atlas and the BCB desk program · v0.1 · 16 September 2026*

*The atlas catalogued 118 ways to cut a text. This document picks from it for one genre and explains the picking. Its central claim is the one you made: the reading must not be agnostic to structure. A comunicado is not a bag of sentences; it is a liturgy with fixed slots, and the slot a sentence sits in changes what every measurement of that sentence means.*

---

# 1. Why structure comes first in this genre

Start with the mechanism, because it decides everything downstream.

An institution that must publish eight times a year, by committee, into a market that prices the text in seconds, converges on a template. The template is not laziness; it is the institution's defence against being misread. Every meeting, the drafters begin from last meeting's text and change what has to change. Three consequences follow.

**First, freedom concentrates in the slots.** The template fixes the skeleton — which paragraphs exist, in what order, doing what job. What varies is the *filler* of each slot: the adjective on *atividade*, the verb of judgement, the intensifier inside a bedrock formula, the presence or absence of a guidance sentence. Variance is therefore not spread evenly across the text; it is concentrated in a small number of predictable positions. A measurement that ignores the skeleton is averaging a few high-signal positions with many locked ones, and dilutes its own signal.

**Second, the same feature means different things in different slots.** Hedge density in the paragraph that describes the world (*a inflação de serviços segue elevada*) reflects uncertainty about data. Hedge density in the paragraph that commits the committee (*antevê ajustes…*) reflects reluctance to promise. The number can be identical; the reading is opposite. Any feature computed over the whole document mixes these, and the mixture is uninterpretable. This is also the statistical point: conditioning on slot is stratification, and stratification is how you remove the variance you already understand so that what is left is the part you want to see.

**Third, the structure itself carries meaning.** Which slot a topic is placed in is a claim about its status. Fiscal policy mentioned in the description of the domestic scenario is background; fiscal policy listed among the risks is a warning; fiscal policy inside the reaction-function sentence is a trigger. The words can be identical across all three. The *address* changed. Slot presence, slot order, and slot migration are therefore measurements in their own right — structure as semantics, in your phrase.

So the design rule is: **segment into functional units first, measure within units, baseline within units, compare across meetings within units, and treat changes to the units themselves as first-class events.**

---

# 2. The structural model — five levels

The reading needs a representation of what a Copom document *is*. Five nested levels, each a real object with its own operations.

| level | object | unit of what | example |
|---|---|---|---|
| **genre** | document type | the whole communicative act | comunicado · ata · RPM/RTI · presser · speech |
| **section** | a titled division the institution itself draws | the institution's own partition | ata sections A–D; RPM chapters and boxes |
| **move** | a functional unit doing one job | *what this stretch is for* | the balance-of-risks paragraph; the vote line |
| **slot** | a fixed position inside a move that takes a variable filler | the locus of choice | the intensifier in *patamar … contracionista*; the guidance verb |
| **span** | the canonical annotation record | offsets on the text | any `{char_start, char_end, dimension, value}` |

Two remarks before the levels are filled in.

The word **move** comes from genre analysis (Swales, *Genre Analysis*, 1990): a stretch of text that performs one recognizable communicative function within a genre, such as *establishing a territory* in a research-article introduction. Moves are the right grain for this genre because the institution itself drafts in moves — a paragraph *is* the balance of risks — and because the market reads in moves ("what did the guidance paragraph say?").

The word **slot** comes from construction grammar and phraseology: a fixed multi-word frame with one or two open positions, the *p-frame* of the Formulary spec. The slot is where the drafter had a choice; the frame around it is what they did not touch. In this genre the slot, not the sentence, is the unit of freedom.

## 2.1 Genre

Each document type has its own move structure and its own natural ground truth. The comunicado *declares and guides*; the ata *narrates and argues*; the RPM/RTI *expounds and models*; the press conference *defends*. These are different jobs and they should not share a baseline. The plumbing already exists — `doc_type` in the span schema — and the rule is that no feature is ever compared across document types without saying so.

## 2.2 Section

The institution's own partition, taken as given because it is the drafters' declared functional map. The ata since the 2017 reform runs in lettered sections with numbered paragraphs — an update of the economic scenario, the scenarios and risk analysis, the discussion of the conduct of monetary policy, and the decision — *the exact titles to be verified verbatim against the fetched corpus and frozen as a test*. The section is a strong prior on the move: the policy-discussion section is where the argument lives, the scenario section where the description lives. The RPM's boxes (*boxes* is the institution's own word) are a distinct section type whose title is itself a function label and whose existence marks where novel analysis entered.

## 2.3 Move — the functional inventory for the comunicado

This is the level you asked about. The comunicado's paragraph liturgy, as observed in the recent corpus, decomposes into eleven moves. Each is named by *what it does*, and each carries its own ground truth — the thing in the world that later tells you whether that move was read correctly.

| move | what it does | typical realization | its own ground truth |
|---|---|---|---|
| **DECLARE** | performs the decision — a Searlean declaration: the rate *is* X because the counting-as authority said so | *O Copom decidiu, por unanimidade, manter a taxa Selic em…* | market surprise vs pre-meeting pricing |
| **DESCRIBE-EXT** | states the external world | *O ambiente externo permanece adverso…* | realized global variables |
| **DESCRIBE-DOM** | states the domestic world: activity, labour market, inflation, expectations | *A atividade… surpreendeu positivamente; as expectativas… permanecem desancoradas* | subsequent prints; Focus revisions |
| **PROJECT** | states the committee's forecasts under stated conditioning assumptions | *As projeções… situam-se em 3,5%… no cenário de referência, com trajetória de juros extraída do Focus* | realized inflation at the horizon |
| **WEIGH** | enumerates and balances risks, typed up/down | *Entre os riscos de alta… Entre os riscos de baixa… O Comitê avalia que o balanço de riscos é assimétrico* | which risks materialized |
| **JUSTIFY** | links the world to the decision: the rationale | *Tendo em vista… o Comitê avalia que…* | consistency with the subsequent ata |
| **COMMIT** | binds the committee's future behaviour: guidance and reaction function | *O Comitê antevê…; os passos futuros dependerão de…; não hesitará em…* | subsequent policy action (SGS 432) — the project's primary ground truth |
| **CONDITION** | attaches escape clauses to commitments | *em se confirmando o cenário esperado; caso julgue apropriado; a menos que…* | whether the condition was invoked |
| **REITERATE** | maintains continuity with prior text — the phatic move | *O Comitê reitera / reforça / relembra que…* | none; its signal is presence and what is *not* reiterated |
| **DEFINE** | restates the framework or glosses its own terms — the metalingual move | *…a convergência da inflação à meta no horizonte relevante, que inclui…* | none directly; changes are regime events |
| **RECORD** | authenticates the document: votes, names, attendance, the next meeting | *Votaram por esta decisão os seguintes membros…* | headcount and dissent are facts |

Three observations that only become visible once the moves are named.

*The comunicado has exactly one performative.* DECLARE is the only move in which the world changes because the sentence was issued. Everything else is commentary on that act or calibration of how far the institution binds itself next time. Speech-act theory names this precisely (Searle's *declaration* versus *assertive* and *commissive*), and the naming turns a vague sense that "the decision sentence is different" into a rule: DECLARE gets its own detectors (the performative apparatus: decision verb, unanimity formula, effective-date clause) and its own ground truth (the surprise against pricing).

*Two of the moves are almost pure form.* REITERATE and DEFINE carry nearly no propositional news; they are Jakobson's phatic and metalingual functions in institutional costume — language whose job is to keep the channel open, and language that talks about its own code. That is exactly why they matter here. A committee that stops reiterating a formula it reiterated for eight meetings has said something by omission, and nothing in a sentiment index will register it. These moves are where the Formulary's stratigraphy and the Redline's deletion chips do their work.

*Ground truth is move-specific.* COMMIT is validated against what the committee later did; PROJECT against what inflation later was; DESCRIBE against the prints; DECLARE against the pricing. The project's headline ground truth — subsequent policy action — is the ground truth *of one move*. Treating it as the truth of the whole document is what makes hawk–dove indices noisy: they score DESCRIBE and WEIGH language against a target those moves were never making a claim about.

## 2.4 Move inventory for the ata

The ata shares most of the comunicado's moves but adds two that are absent from the shorter text and change its character.

**NARRATE** — the ata is reported discourse about the committee's own meeting, almost entirely at the indirect end of the Leech–Short speech-presentation scale (*o Comitê discutiu…; avaliou-se que…; observou-se divergência…*). The ata's function is to convert a meeting (the *fabula*, the events as they happened) into an account (the *syuzhet*, the order and manner of their telling), and how directly it reports is a transparency dial the institution controls.

**ATTRIBUTE** — the ata assigns positions to sub-groups: *a avaliação predominante foi…; outro grupo…; parte dos membros…*. This is the BCB's quantifier ladder, far thinner than the FOMC's *some/several/many participants* calculus, and the thinness is the baseline: when it thickens (June 2023's recorded divergence; May 2024's two argued camps), the committee is publishing its own disagreement.

The policy-discussion section of the ata is also where JUSTIFY expands from a sentence into an argument. That is the natural home of the Toulmin layout, the argumentation schemes, and thematic progression — axes that are wasted on the comunicado's compressed rationale and earn their cost only where the institution actually argues.

## 2.5 Slot

Below the move, the slot. A slot is a position inside a formula that has taken more than one filler over the corpus history, or that the register plainly licenses to vary. The Formulary induces slots mechanically (p-frame induction over lexical bundles); the desk names the ones that matter. The high-value slots in the comunicado, from the desk program:

- the **intensifier** and the **nominal** in the restraint formula: *patamar / território* … *{suficientemente, significativamente, ainda mais}* … *contracionista*
- the **virtue pair** in *a conjuntura demanda {X} e {Y}* — two slots over a five-member paradigm (*serenidade, parcimônia, cautela, paciência, prudência*)
- the **guidance verb** and its **modifiers**: *{considera, entende, avalia, julga, antevê}* … *{como provável, neste momento, em se confirmando…}*
- the **magnitude filler**: *a magnitude total do ciclo … será {ditada pelo firme compromisso | estabelecida à luz de novas informações}*
- the **epithet on a fixed referent**: *atividade {resiliente, aquecida, robusta}; expectativas {desancoradas, …}*
- the **membership of the determinant list** in the reaction-function sentence
- the **count and horizon numerals** in guidance: *nas próximas {duas} reuniões*

Each is a small categorical variable with a dated history, and each is a place where a lexical diff reports "one word changed" and a slot-aware diff reports "the commitment moved a rung."

---

# 3. The move × axis matrix — which axes to use, per structure

This is the direct answer to *which axes*. It is a matrix, not a list, because the honest answer depends on the move. Axes are cited by their atlas entry number and, where one exists, the registry dimension; ◆/◇/○ is the atlas's detectability temperament (countable today · operationalizable with work · interpretive tradition). Rows are the comunicado's moves; the ata's NARRATE and ATTRIBUTE are added at the bottom.

| move | primary axes (build first) | secondary axes | do not bother |
|---|---|---|---|
| **DECLARE** | performative apparatus — decision verb, unanimity formula, effective-date clause (atlas 4.2) ◆ · numerals and granularity (E04) ◆ · paratext: unanimity, headcount (atlas 12.10) ◆ | — | stance, hedging (there is none) |
| **DESCRIBE-EXT / DOM** | epithet ledger — the modifier on a fixed referent (Appraisal Graduation, atlas 8.1; Focus, E20) ◆ · aspect and phasal periphrasis — *vem cedendo* vs *cedeu* (E19) ◆ · focalization and unowned surprise — *surpreendeu* with no experiencer (E09) ◇ · transitivity process types — material vs relational vs mental (atlas 2.3) ◆ | semantic prosody of *fiscal, câmbio* inside the register (atlas 3.10) ◆ · entity grid across meetings (atlas 5.6) ◆ | argumentation (nothing is argued here) · guidance verbs |
| **PROJECT** | granularity and rounding (E04) ◆ · conditioning-assumption slots — the Focus path, exchange rate, oil premises as p-frames ◆ · estimative probability where the projection is verbal (atlas 9.10) ◆ | horizon deixis — *no horizonte relevante* and what it currently means (atlas 5.7) ◆ | appraisal (projections are flat by design) |
| **WEIGH** | list membership and order diff (slot grammar; genetic criticism, atlas 12.5) ◆ · asymmetry lexicon and its intensifiers (atlas 8.1) ◆ · frame elements — which risks name an agent, which a magnitude (E03) ◇ | argument from consequences (E08) ◇ · force dynamics on the restraint metaphors (atlas 3.7) ◇ | thematic progression (lists have none) |
| **JUSTIFY** | warrant explicitness — Toulmin slots (E07) ◇ · argumentation schemes (E08) ◇ · causal connectives (r4) ◆ · epistemic verb ladder — *considera < entende < avalia < julga < antevê* (r3; desk program §2.1) ◆ | thematic progression (E06) ◇ · at-issueness of the premises (E01) ◇ | — |
| **COMMIT** | modality ladder, with the *dever* disambiguation supervised by the official English (E14) ◆ · guidance verb × hedge stack ◆ · commitment type: data-dependent / conditional / commitment (Plate 11) ◇ · magnitude-frame slot ◆ · determinant-list diff ◆ · estimative probability (atlas 9.10) ◆ | praeteritio in the press-conference twin (E05) ◇ · commissive strength (atlas 4.1) ◇ | description features (nothing is described) |
| **CONDITION** | subjunctive density and the *caso / se / a menos que / em se confirmando* frames (r3; E07 rebuttal slot) ◆ · rebuttal rate (E07) ◇ | at-issueness — is the condition projective or asserted? (E01) ◇ | — |
| **REITERATE** | bedrock share and formula age (E16 stratigraphy) ◆ · deletion events weighted by age (Redline) ◆ · phatic verb inventory — *reitera, reforça, relembra* (Jakobson, atlas 1.5) ◆ | — | anything content-based |
| **DEFINE** | metalingual markers (atlas 1.5) ◆ · framework-term stability (Formulary status) ◆ · translation shift on defined terms (E14) ◆ | code selection — borrowing vs calque for the defined term (E15) ◆ | — |
| **RECORD** | vote-line fields: unanimity, headcount, dissent direction ◆ · next-meeting mention ◆ · release timing per language (atlas 12.12) ◆ | diplomatics anatomy check — is the closing authentication block intact? (atlas 12.3) ◆ | everything linguistic |
| **NARRATE** *(ata)* | speech-presentation scale — direct / indirect / report of a speech act (atlas 6.5) ◇ · reported-discourse framing verbs (atlas 7.3) ◆ · impersonal *-se* rate (r3) ◆ | Goffman footing — who is principal in each reported clause (atlas 7.1) ◇ | — |
| **ATTRIBUTE** *(ata)* | quantifier ladder and group-attribution rate (`attr`; desk program §2.4) ◆ · attribution thickness as a z-score against its own thin baseline ◆ | stance triangle — alignment between attributed groups (atlas 7.4) ◇ | — |

Read the matrix column-wise and a pattern appears: the *primary* column is almost entirely ◆ — counting, lexicon, parser class, local, deterministic. The classifier-grade axes (Toulmin, argumentation schemes, focalization, speech presentation) cluster in JUSTIFY, NARRATE, and the press conference, which is where the institution stops templating and starts composing. That is the cost-allocation rule in one sentence: **spend deterministic effort on the templated moves and classifier effort on the composed ones.**

Read it row-wise and the second pattern appears: the *do not bother* column is not empty. Stance in DECLARE, appraisal in PROJECT, argumentation in DESCRIBE — these are not merely low-yield; they are category errors, and a pipeline that runs every axis over every sentence will produce confident numbers for them. Structure-first is as much about what *not* to compute as about what to compute.

---

# 4. Functions — three grains that must not be collapsed

You asked what functions to break things into. There are three functional taxonomies in play, at three grains, and the registry will drift if they are conflated.

**Grain 1 · genre move** (Section 2.3 above) — *what job this paragraph does in the liturgy*. Eleven values for the comunicado, thirteen for the ata. Assigned by segmentation, mostly rule-based: the moves have positional and lexical signatures strong enough that a small grammar plus a formula lexicon segments a comunicado deterministically.

**Grain 2 · clause function** — the existing `func` dimension from the functional reader: *framing · assessment · rationale · caveat · signpost · decision · pivot · coda*. This is the rhetorical role of a clause *within* a move. A WEIGH paragraph contains framing clauses, assessment clauses, and a pivot; a COMMIT paragraph contains a decision clause wrapped in caveats. Keep `func` at this grain; do not stretch it upward to cover moves.

**Grain 3 · illocutionary class** — Searle's five: assertive, directive, commissive, expressive, declaration. This is *what kind of act* a sentence performs, orthogonal to both grains above. DECLARE contains one declaration and possibly an assertive; COMMIT contains commissives; nearly everything else is assertive; the comunicado contains no directives and no expressives, and that absence is a genre fact worth recording.

The three compose. A sentence is, simultaneously, *in* a move, *doing* a clause function, and *performing* an act. The registry should carry them as three dimensions — `move` (new, document-structure grain), `func` (existing), `act` (new, sentence grain) — and slot-conditioning is then a containment join: every other span inherits the `move` of the paragraph it sits in.

Two further functional lenses inform the move inventory without becoming dimensions of their own. Jakobson's six functions (referential, emotive, conative, phatic, metalingual, poetic) explain *why* REITERATE and DEFINE exist and why the coordinated virtue pair reads as poetic form; they are the theory behind the inventory. Swales's move analysis supplies the method for deriving the inventory from a corpus rather than asserting it: read fifty comunicados, mark what each paragraph is for, cluster, name — and then verify that the eleven names above are what the clustering returns. That verification is a test, not an assumption.

---

# 5. Structures — the representations the reading runs on

"Structure" in the computational sense: what objects the system holds so that the functional decomposition is operational rather than decorative.

## 5.1 The genre grammar

A small formal grammar per document type, stating which moves exist, their order, their cardinality, and their optionality. For the comunicado, roughly:

```
comunicado ::= DECLARE
               DESCRIBE-EXT
               DESCRIBE-DOM
               PROJECT
               WEIGH
               JUSTIFY
               COMMIT?          -- guidance is optional; its presence is a traded binary
               CONDITION*       -- attaches to COMMIT; may be absent
               REITERATE*
               DEFINE?
               RECORD
```

The grammar does three things. It drives segmentation (a parser assigns moves with the grammar as prior). It defines *structural events*: a move added, removed, reordered, split, or merged is a violation of last meeting's parse and is logged as a `dimension=structure` span with the event type as value. And it makes the *optional* moves explicit — the market already counts whether the guidance sentence is present; the grammar makes that a field, not a reading.

## 5.2 Moves as spans

No new table. A move is a span record with `dimension="move"`, `value=<move name>`, covering a paragraph or a sentence range. Slot-conditioning is a join on containment: any feature span whose offsets fall inside a move span inherits that move. This keeps the canonical schema untouched — a MINOR bump under the Codex's versioning rules — and makes "hedge density in COMMIT" a SQL `WHERE` clause rather than a new pipeline.

## 5.3 Slot-conditioned baselines

The baseline reference for every z-score is keyed by `(doc_type, move, era)` rather than by document type alone. This is the concrete form of "measure within units, baseline within units." It also changes the Null Reader's conditioning: surprisal on a bedrock REITERATE formula measures the model's fit to the register; surprisal in JUSTIFY measures the text. Pooling them, which the current spec does, mixes two quantities that should be reported separately.

## 5.4 Slots as p-frames with paradigms

The Formulary already carries this: `formulary` rows are frames, `formula_slots` rows are the attested fillers with dates. What structure-first adds is a `move` column on the frame — formulas live in moves, and the paradigm of the restraint intensifier is a COMMIT/JUSTIFY paradigm, not a corpus-wide one. Ghost words are then proposed from the right paradigm.

## 5.5 The alignment cascade

The diff — the project's first-class object — should align top-down through the structure: **move to move first, then paragraph to paragraph within the move, then sentence to sentence within the paragraph.** This does two things a flat sentence alignment cannot. It handles reordering (a paragraph that moved from DESCRIBE-DOM to WEIGH aligns to nothing at its old address and is reported as a *migration*, not as a deletion plus an insertion). And it makes the Redline's chips move-aware: a deleted sentence in REITERATE (a formula retired) is a different chip from a deleted sentence in DESCRIBE (a fact no longer worth stating).

## 5.6 The structural event ledger

Five structural operators apply to every move, uniformly, giving structure its own five-feature signature per meeting:

| operator | question | example event |
|---|---|---|
| **presence** | is the move there? | guidance sentence absent for four meetings, then present (December 2024) |
| **position** | where in the order? | fiscal content's address moves from DESCRIBE-DOM to WEIGH (the August 2026 desk reading) |
| **share** | how much of the document? | the ata's policy-discussion section grows as a share of tokens — the committee argued longer |
| **filler** | which paradigm member occupies each slot? | virtue pair *serenidade e cautela* replaces *serenidade e parcimônia* |
| **delta** | what changed in the above since the prior meeting? | a bedrock formula mutated: *suficientemente* → *adequadamente* |

These are counting-class, deterministic, and local. They are also the features most likely to survive the referee regression, because they are the ones the market already reads by hand — the desk program's observation that practitioners already read the comunicado as a diff of moves, without the vocabulary for it.

---

# 6. A specimen — one comunicado, parsed

Pastiche, in register, segmented into moves with the primary axes that fire in each. This is what the segmentation layer should hand to every downstream instrument.

> **[DECLARE]** *O Copom decidiu, por unanimidade, manter a taxa Selic em 14,00% a.a.*
> → performative apparatus complete; unanimity present; granularity level 0,25.
>
> **[DESCRIBE-EXT]** *O ambiente externo permanece adverso, caracterizado por incerteza elevada e volatilidade nos ativos.*
> → epithets *adverso, elevada*; no agent; relational process.
>
> **[DESCRIBE-DOM]** *No cenário doméstico, a atividade econômica surpreendeu positivamente, e as expectativas de inflação permanecem desancoradas.*
> → unowned surprise (no experiencer); epithet *desancoradas* stable for several meetings; continuative aspect *permanecem*.
>
> **[PROJECT]** *As projeções de inflação do Copom situam-se em 3,5% para 2026 e 3,2% para o primeiro trimestre de 2027 no cenário de referência.*
> → granularity 0,1 on both; horizon named; conditioning frame present.
>
> **[WEIGH]** *Entre os riscos de alta, destacam-se a desancoragem das expectativas e a resiliência da demanda. Entre os riscos de baixa, uma desaceleração mais acentuada da atividade global. O Comitê avalia que o balanço de riscos é assimétrico.*
> → list membership: 2 up / 1 down; *assimétrico* with no direction stated (an unfilled frame role); ordering unchanged from the prior meeting.
>
> **[JUSTIFY]** *Tendo em vista os efeitos defasados da política monetária e a persistência das expectativas desancoradas, o Comitê avalia que a manutenção da taxa por período prolongado é a estratégia adequada.*
> → Toulmin: two data, warrant explicit (*tendo em vista*), qualifier *avalia*, no rebuttal in this sentence; epistemic verb mid-ladder.
>
> **[COMMIT]** *O Comitê seguirá vigilante e avalia que a estratégia de manutenção por período prolongado permanece adequada. Os passos futuros dependerão da evolução da dinâmica inflacionária, das expectativas de inflação e do balanço de riscos.*
> → commitment type: data-dependent; determinant list: 3 members (one fewer than the prior meeting — *hiato do produto* absent: a membership event); no *antevê* — guidance sentence absent.
>
> **[CONDITION]** *O Comitê não hesitará em retomar o ciclo de ajuste caso julgue apropriado.*
> → subjunctive *julgue*; *caso* frame; litotes *não hesitará* carrying commitment work.
>
> **[REITERATE]** *O Comitê reitera que perseverará até que se consolide a convergência da inflação à meta.*
> → bedrock formula, age 12 meetings, intact.
>
> **[RECORD]** *Votaram por esta decisão os seguintes membros do Comitê: …*
> → headcount 7; unanimity consistent with DECLARE; next meeting not named.

Every arrow annotation is computable from the primary column of the matrix in Section 3. The two most interesting findings in this specimen — the missing *hiato do produto* in the determinant list and the absent guidance sentence — are both *structural* events that no lexical index would rank above the adjectives.

---

# 7. Build implications — what changes in the plan

Nothing in the existing tiers is reordered; two things are inserted.

**Segmentation becomes Tier 1, before the Formulary.** The move grammar and a rule-based segmenter (positional prior + formula lexicon + a small classifier for the residue) run right after the encode gate. Everything downstream — Formulary induction, baselines, the Redline's alignment cascade — keys on `move`. Building the Formulary corpus-wide first and adding moves later would mean re-inducing paradigms; do it once, in the right order.

**Three registry additions, all MINOR.** A `move` dimension (document-structure grain, closed set per `doc_type`); an `act` dimension (sentence grain, Searle's five); a `structure` dimension carrying the five structural-event operators. `func` stays as it is. Baseline references gain a `move` key.

**The first thing to look at once segmentation runs:** the move-share and move-presence time series over the whole backfill, one chart per move. Before any linguistic feature is computed, that chart should already show the 2017 reform, the 2020 forward-guidance episode, the guidance droughts, and — the prediction to check — a rise in JUSTIFY share ahead of pivots. If it does not show those, the segmenter is wrong, and it is cheaper to learn that before the classifiers are trained.

---

# 8. For sumi — the concepts, consolidated

**move** — a stretch of text that performs one recognizable function within a genre (Swales 1990). In the comunicado, roughly a paragraph: DECLARE, DESCRIBE, PROJECT, WEIGH, JUSTIFY, COMMIT, CONDITION, REITERATE, DEFINE, RECORD; the ata adds NARRATE and ATTRIBUTE. The unit the institution drafts in and the market reads in.

**slot** — a position inside a fixed formula that takes a variable filler; the *p-frame* of phraseology. The unit of drafting freedom in a templated genre. Its history of fillers is the attested paradigm (Saussure's paradigmatic axis made queryable).

**three functional grains** — genre move (what job the paragraph does), clause function (rhetorical role within the move: framing, assessment, rationale, caveat, signpost, decision, pivot, coda), illocutionary class (what act the sentence performs: assertive, commissive, declaration…). Orthogonal; keep as three dimensions.

**declaration** (Searle) — an utterance that changes the world by being issued by the right authority in the right context (*X counts as Y in C*). The comunicado's DECLARE move is its only declaration; everything else is commentary or calibration.

**phatic / metalingual** (Jakobson) — language whose job is to keep the channel open, or to talk about the code itself. REITERATE and DEFINE are these functions in institutional form; near-zero propositional news, high signal by omission.

**slot-conditioned baseline** — a z-score whose reference population is keyed by (document type, move, era), not by document type alone. The operational form of "measure within units."

**structural event** — a change to the skeleton rather than to a filler: move added, removed, reordered, split, merged; slot filled or emptied. Logged as a span; the market already reads these by hand.

**alignment cascade** — diffing top-down through the structure: move → paragraph → sentence. Handles reordering as *migration* rather than deletion-plus-insertion.

**function migration** — a topic changing its move address (fiscal from DESCRIBE to WEIGH). Meaning carried by position, with the words unchanged.

**move-specific ground truth** — COMMIT validates against subsequent action; PROJECT against realized inflation; DESCRIBE against the prints; DECLARE against pricing. The project's headline ground truth is the truth of one move.

**eschatocol** (diplomatics) — the closing authentication block of an institutional document; the vote line is the comunicado's. Boilerplate is the authentication layer, not noise.

*— End of Structure First v0.1 —*
