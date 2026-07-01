---
date: 2026-06-28
title: How Does "Google Translate" Know Which Language I Typed?
summary: Classical algorithms from before the age of artificial intelligence
description: Classical algorithms from before the age of artificial intelligence
slug: google-translate
translationKey: google-translate
---

Problems used to require elegant solutions. We were constrained by limited hardware and the lack of readily available answers. You couldn't simply throw an AI model at every problem and expect a polished result. In fact, even today that wouldn't be practical—you'd quickly run out of tokens. Programmers had to earn the title of engineer by inventing clever algorithms that solved problems efficiently. A few days ago, I found myself diving into one particularly fascinating example.

Have you ever wondered how "Google Translate" knows which language you've entered? Today the obvious answer is "AI." But even now, the input first goes through a series of simple yet remarkably effective statistical checks. The classical approach is based on **n-grams**. To save you a trip to Wikipedia, here's the definition:

> An n-gram is a sequence of n elements, which may be sounds, syllables, words, or letters, depending on the context.

Let's split the word `Hello` into character bigrams. The prefix bi- means two, so we're looking for every pair of consecutive characters. There are four of them: `he`, `el`, `ll`, and `lo`. N-grams come in several forms:

| Type               | Examples                   |
| ------------------ | -------------------------- |
| Character bigrams  | `th`, `er`                 |
| Word bigrams       | `to the`, `in the`         |
| Character trigrams | `the`, `ing`               |
| Word trigrams      | `one of the`, `as well as` |

Every language has its own characteristic patterns. Unigrams and bigrams overlap too much between closely related languages. Quadrigrams are more accurate but require considerably more storage. Trigrams hit the sweet spot: they're usually enough to distinguish Spanish from Italian with remarkable accuracy. In general, longer sequences improve precision, although using more than three consecutive elements rarely provides a meaningful benefit. Some specialized applications, however, do make use of even longer sequences.

The algorithm doesn't try to understand the meaning of the text—it simply looks for statistical fingerprints. This becomes especially useful for languages such as Chinese, where words aren't separated by spaces. The text is one continuous stream of characters, making the concept of a "word" much less defined than in most Western languages. Fortunately, the algorithm doesn't care. It simply breaks the text into n-grams and analyzes their frequencies.

One amusing consequence of language detection occasionally appears on Twitter. Every now and then I'd come across tweets that looked Russian at first glance, yet something felt off. The letters were Cyrillic, but not quite the Cyrillic I was used to. It turned out to be **Bulgarian Cyrillic**, a modernized form of the script that emphasizes the distinct identity of Bulgarian writing while visually bridging the gap between Latin and traditional Russian typography.

Russian and Bulgarian are a textbook nightmare for statistical language detection. At the level of bigrams and trigrams, the two languages are so similar that a short sentence can easily fool the algorithm.

Both languages use the Cyrillic alphabet, so any detector that first groups languages by their writing system naturally places them in the same bucket. Bulgarian also lacks uniquely identifying letters that Russian doesn't have (unlike Serbian, which includes characters such as `ђ`, `ћ`, and `џ`). On top of that, the two languages share a huge number of common character trigrams.

Still, a few reliable markers help separate them. One of the most important is the letter `ъ`. In Russian it appears only rarely, while in Bulgarian it's a fully fledged vowel used constantly. Conversely, the presence of `ы` immediately rules out Bulgarian, since that letter simply doesn't exist in the language.

For short tweets, however, the classifier isn't always confident enough to make the right call. When that happens, Twitter may assign the HTML attribute `lang="bg"`, causing the browser to render the text using Bulgarian Cyrillic.

![Twitter](twitter.png)

N-grams are incredibly useful. They're widely used in predictive text systems, where a keyboard suggests the next word based on the words you've already typed. The same idea also powers optical character recognition. When a computer scans a poorly printed document, it may be uncertain about a particular character. For example, the lowercase `l` can easily be mistaken for the digit `1` or the uppercase `I`. Instead of making a blind guess, the OCR engine looks at the surrounding character n-grams. If the preceding letters are `w` and `i`, it will most likely choose `l`, because the trigram `wil` (as in "will") is extremely common in English, whereas `wi1` simply doesn't occur in natural text. The same statistical principle is used in speech recognition systems.

N-grams have also found an important role in bioinformatics DNA can be thought of as one enormous uninterrupted string composed of only four "letters"—the nucleotides `A`, `T`, `C`, and `G`. Just like Chinese, DNA contains no spaces, so n-grams (known in biology as **k-mers**) became one of the fundamental tools for analyzing genetic sequences.

In cybersecurity, n-grams are used for both signature-based and heuristic malware detection. Instead of letters or words, the algorithm analyzes sequences of bytes or system calls. Early spam filters relied on the same idea. Phrases such as "click here" or "buy now," combined with suspiciously frequent uppercase character patterns, were often enough to send an email straight to the spam folder.

Another fascinating application appears in attempts to uncover the identity of Bitcoin's mysterious creator, Satoshi Nakamoto. Satoshi left behind a considerable amount of writing on early cryptocurrency forums, giving researchers plenty of material to analyze. [One article published by The New York Times](https://www.nytimes.com/2026/04/08/business/bitcoin-satoshi-nakamoto-identity-adam-back.html) applied **stylometry**—the statistical analysis of writing style—to those posts. Its author argued that Adam Back might in fact be Satoshi Nakamoto. Among the clues were several highly unusual habits shared by both writers: they made the same rare mistakes when using hyphens and alternated between British and American spelling in remarkably similar ways. None of these details proves anything on its own, but together they form the kind of statistical fingerprint that stylometry is designed to detect.

I've always had a soft spot for elegant algorithms like these. They remind us that some of the most beautiful ideas in computer science don't require enormous neural networks or supercomputers. Sometimes all it takes is a clever observation, a bit of mathematics, and an engineer willing to think creatively.
