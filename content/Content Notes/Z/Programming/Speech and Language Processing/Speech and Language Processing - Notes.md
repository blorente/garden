---
publish: true
created: 2026-06-15T20:26:12.167+01:00
modified: 2026-06-15T20:26:11.849+01:00
---

Links: [[Content Notes/Z/Programming/Speech and Language Processing/Speech and Language Processing - Notes]], [[Reading List]]
Date: 2026-06-15
Visibility (remove one):

- [[Public]]

---

# Speech and Language Processing

# 1: Words and Tokens

- This is from the notes of the first chapter: https://web.stanford.edu/~jurafsky/slp3/slides/tokens\_jan26.pdf
- **Vocabulary** (V): Set of possible parts of speech in a particular body. The body could be "The works of shakespeare", "this sentence", or "the English Language (as understood by a dictionary or something)".
- A text has a set of **Types**, which is the set of symbols it can have.
  - We can redefine Vocabulary as "The set of types in a given corpus".
  - So if the vocabulary is V, which is a set of all different types, then |V| is very relevant.
- and a set of **Instances**, which is the number of occurrences of types, either grouped by type or just added together. When added together, we use N.
- Types could be words, but not necessarily. For instance, how do you break up a Chinese hanzi sentence is not agreed upon, and Japanese is also hard for the same reason (no spaces!)

So many words. Usually, |V| = kN^b , so the number of types increases as N (the total number of instances) increases.

- So, because words are very very hard, and they increase with the length of the text to untenable degrees, we don't use them.

So we tokenize. We use subowrds that follow different rules.

- For instance, " she's" (including the leading space) may be a token we consider, whereas we'd split "Alice's" into "Alice" and "'s".

- This gives us the power to **bound the universe of tokens:**
  - Every letter
  - Every combination of two letters
  - Clitics ("'s").
  - Every set of 1, 2 or 3 digits.
  - And any bags of common words we want to add on top.
    - Like " she's", or proper nouns

- That gives us graceful degradation: If we don't know  a word, we could always devolve it into individual pairs of letters for instance.

- Aside: Spanglish is a problem for text recognition, and that's hilarious :D

- Other aside: Corpus (corpi? [Corpora](https://en.wiktionary.org/wiki/corpus)!) are inherently biased, so we must investigate the motivations and methods of its creation.

- Then there's a tutorial about regex...

- Work on NLP.
  - We can improve along two axes:
    - Match more things that we should have matched (increase **coverage**, or **recall**).
    - Stop matching things that we shouldn't have matched (increase **accuracy** or **precision**).

  - Then we go into the actual meat of tokenization, which is not covered in the slides so I'll read the book.

### Sources

- https://web.stanford.edu/~jurafsky/slp3/
- https://web.stanford.edu/~jurafsky/slp3/ed3book\_jan26.pdf
