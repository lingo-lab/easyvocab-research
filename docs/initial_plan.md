### Outline

1. Dataset Collection (cambridge scraping)
    * format: word, defs, examples
    * 200x words, sampled based on my b1 deck (uniform: verb, noun, adj/adv, other (e.g. phrase))
2. target task: de dict (given de word, generate def & examples)
    * llm judge setup
    * exps: llm judge quality, different models, benchmark alignment
3. transfer: context based dict


#### Target Task

data: cambridge

evaluation
* def
    * alignment (@1, @any): does the definition coincide with any of those from the dictionary?
    * simplicity: pairwise | optional
        * given two definitions, which definition is easier to understand
        * simpler alternative: instruction following -- definition is constructed of simple words, short, easy to understand
* example
    * def alignment: sentence(word) == sentence(def)
    * naturalness: word used in natural context (no (e.g. wrong form or wrong preposition), yes)
    * instruction following (e.g. sentence is short | rule-based <8 words)
    * grammar: sentence is gramatically correct
* translation quality (both def / sentence)
    * word: natural / doesn't need clarifications
    * natural + uses the word + conveys the meaning

llm as a judge
* hillclimbing
    * 10% of data; take some baseline prompt -> generate & evaluate yourself (note: special focus on naturalness / clarity / etc.)
* human alignment
    * take non-obvious metric (e.g. naturalness) and ask Colin to annotate | check agreement / cohen's kappa

downstream task:
* choose ~20 diverse models (budget: 2 input, 10 output)
    * frontier & (low vs medium reasoning)
    * multi-lingual focused
    * open weight
* run evaluation
    * important: observability (everything: time / cost / etc.)

bench alignment
* propose hypothesis based on papers & find benchmarks
    * benchmark choice criteria: aligns with hypothesis, contains at least 15 models our of 20 used above
* compute: spearman corr, position regret, kendall's tau for each

#### Further experiments

bench alignment for other languages
* pick best correlated benches from above
* construct similar dataset for el (greek), ru (russian)
    * note: el -- low resource language, ru -- is high resource (approx like de)
* for models(???), same eval approach
    * model selection: choose models from some bench-s that align best with our best benches but in the target language, sample the models from there
    * evaluate & check correlation


in-context transfer
idea: now we need to generate de dict, but given some context
* dataset: sample some sentences from some book
* word + context (8 words on left/right)
* add new metric (instead of @any) definition alignment
* test that models perform in accordance

big questions:
* how do we account for noise? (bootstrapping?)
* ...







### Helpful Sources


Papers:
* The Bitter Lesson Learned from 2,000+ Multilingual Benchmarks
* Towards Multilingual LLM Evaluation for European Languages

Main insight: which benchmarks correlate better with target task / human preference
note: MMLU performance correlation

Lecture: [stanford | llm eval](https://www.youtube.com/watch?v=uoSynlTDyMs)
