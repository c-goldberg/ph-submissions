---
title: "Introduction to Text Analysis for Non-English and Multilingual Texts"
slug: non-english-and-multilingual-text-analysis
layout: lesson
collection: lessons
date: YYYY-MM-DD
authors:
- Ian Goodale
reviewers:
- Forename Surname
- Forename Surname
editors:
- Laura Alice Chapot
review-ticket: https://github.com/programminghistorian/ph-submissions/issues/612
difficulty:
activity:
topics:
abstract: Short abstract of this lesson
avatar_alt: Visual description of lesson image
doi: XX.XXXXX/phen0000
---

{% include toc.html %}

## Lesson Goals

This lesson will provide a solid introduction on how to begin analyzing a corpus of non-English and/or multilingual text using Python. We will go over the fundamentals of three commonly used packages for performing text analysis and natural language processing (NLP): the Natural Language Toolkit (NLTK), spaCy, and Stanza. We’ll review and compare the core features of the packages so you can become familiar with how they work and learn how to discern which tool is right for your specific use case and coding style.

We will also go through practical code examples that show how to perform the same tasks using each package: how to split a multilingual text into sentences and its component languages, detect the language of each sentence, and perform additional processing and analysis on each sentence as needed.

## Basics of Text Analysis

Computational text analysis is a broad term that encompasses a wide variety of approaches, methodologies, and tools that can be used to work with and analyze digital texts, from small-scale corpora and single texts to large amounts of textual data. Harnessing computational methods allows you to quickly perform tasks that are far more difficult to do without computational methods, such as:

- analyze very large corpora that would be too large to reasonably examine by hand
- quickly, easily analyze grammatical structures within a text
- efficiently generate visualizations representing characteristics of texts or corpora
- the meaning of patterns of frequency and distribution
- the function of syntactic units in texts
- the development of an author’s style over time
- and many more

Working with computational methods can also be used to support to support more traditional text analysis methods and techniques–for example, to aid in the close reading of a text by quantitatively analyzing its grammatical structures or peculiarities. Working with and developing an understanding of computational methods for analyzing a text can be its own reward, as well, even if its not something you plan to use in your professional or academic work. Developing skills in textual analysis both expands your skillset and opens up new avenues for exploration and understanding of a text, allowing you to experience new ways of looking at and thinking about textual information.

## Working with Non-English and Multilingual Text

There are a number of things to consider when working with computational analysis of non-English text, many of them specific to the script your language(s) are written in.

- Encodings
  - Text can be encoded in different ways, allowing computers to read, transform, and work with its characters. Unicode is the most commonly used encoding standard, and was designed to handle text from all of the world’s major writing systems. UTF-8 is one of the most common Unicode encodings, and the one that Python defaults to using for text. In this tutorial, we will be using UTF-8 by default when we work with our text in Python.
  - Other encodings you may want to be familiar with are ASCII–a subset of Unicode that only contains 128 characters, and doesn’t support scripts outside of standard Latin characters used in English–and other Unicode encodings beyond UTF-8, such as UTF-32. For most people’s purposes, however, working within UTF-8 will be sufficient.
- Right-to-left vs Left-to-right script
  - Working with scripts that read right-to-left can sometimes pose issues when processing text; there are special libraries that exist to support this (e.g. [Arabic Reshaper](https://github.com/mpcabd/python-arabic-reshaper/tree/master), written in Python).
- Character-based languages often have properties that are not supported by many existing tools; the way Chinese handles word boundaries, for example, is very different than alphabet-based languages spoken in Europe. While there are tools specially made to navigate the properties of these languages, you may have to adjust your approach and workflow to suit the individual needs of the language(s) you are working with.
- Support from already existing tools for non-English languages is often lacking, but is improving in quality with the introduction of a greater quantity of high quality models for processing other languages. Still, many tutorials and tools you encounter will default to or emphasize English-language compatibility in their approaches.
- Detecting and engaging with the different languages in a single text is another issue that can be difficult to navigate, but we’ll go through some simple examples of how to do this in this tutorial. In your own work, it’s always best to thorough think through an approach before applying your methods to the text, considering how that approach suits your personal research or project-based needs. Being flexible and open to changing your workflow as you go is also helpful.

## Tools We’ll Cover

### The Natural Language Toolkit (NLTK)

- NLTK is a suite of libraries for building Python programs to work with language data. Originally released in 2001, NLTK has excellent documentation and an active, engaged community of users that make it an excellent tool to learn and experiment with when beginning to work with text processing. More advanced users will find its wide variety of libraries and corpora useful, as well, and its structure makes it very easy to integrate into one’s own pipelines and workflows.
- It supports different numbers of languages for different tasks (it contains lists of stopwords for 23 languages, for example, but only has built-in support for word tokenization in 18 languages).

### spaCy

- spaCy has built-in support for a greater variety of languages than NLTK, with models of differing levels of complexity available for download. If you want to save time on processing speed you can use a smaller, less accurate model on a simple text, for example, rather than a more complex model that may return more accurate results.
- spaCy is known for its high speed and efficient processing, and is often faster than NLTK and Stanza.
- Overall, spaCy focuses more on being a self-contained tool than NLTK. Rather than integrating NLTK with a separate visualization library such as [matplotlib](https://matplotlib.org/), for example, spaCy has its own visualization tools, such as [displaCy](https://demos.explosion.ai/displacy), that can be used in conjunction with its analysis tools to visualize your results.

### Stanza

- While often slower than NLTK and spaCy, Stanza has language models available not accessible through the other libraries. The package contains pretrained neural models supporting [70 languages](https://stanfordnlp.github.io/stanza/models.html#human-languages-supported-by-stanza). A full list of its models can be viewed [here](https://stanfordnlp.github.io/stanza/performance.html).
- Stanza was built with multilingual support in mind from the start, and working with text in different languages feels very intuitive and natural with the library’s syntax.
- Running a pipeline on a text is extremely simple, and allows you to access various aspects of a text—for example, parts-of-speech and lemmas—with minimal coding.

To summarize, all three packages can be very effective tools for analyzing a text in a non-English language (or multiple languages), and it’s worth investigating each package’s syntax and capabilities to see which one best suits your individual needs for a given project.

## Sample Code and Exercises

We will now go through sample code to demonstrate performing the same tasks using each package. We will take a corpus of text from _War and Peace_ in the original Russian, which contains a substantial amount of French text, and show how to split it into sentences, detect the language of each sentence, and perform some simple analysis methods on the text.

The text file we will be using (sourced [from Wikipedia](https://ru.wikisource.org/wiki/%D0%92%D0%BE%D0%B9%D0%BD%D0%B0_%D0%B8_%D0%BC%D0%B8%D1%80_%28%D0%A2%D0%BE%D0%BB%D1%81%D1%82%D0%BE%D0%B9%29/%D0%A2%D0%BE%D0%BC_1)) contains an excerpt from the first book of the novel, and can be downloaded from this lesson's [assets folder here](https://github.com/programminghistorian/ph-submissions/blob/gh-pages/assets/non-english-and-multilingual-text-analysis/war-and-peace-excerpt.txt). This is the only textual resource you'll need to go through the lesson.

### Loading and Processing a Text

First, let's load our text file so we can use it with our analysis packages.


```python
# open the file and assign it to the variable named "war_and_peace" so we can reference it later on
# then, print the contents of the file to make sure it was read correctly

# we are using a minimally pre-processed excerpt of the novel for the purposes of this tutorial
with open("sample_data/war_and_peace_excerpt.txt") as file:
    war_and_peace = file.read()
    print(war_and_peace)
```

Now, let's clean out the newline characters to make the text easier to work with computationally. We won't worry too much about thoroughly cleaning the text for the purposes of this tutorial, since we will primarily focus on our analysis methods rather than pre-processing. For a good introduction to preparing your text for multilingual analysis, please consult [this article](https://modernlanguagesopen.org/articles/10.3828/mlo.v0i0.294).


```python
# replacing newlines ("\n") with a space, assigning the cleaned text to a new variable, then printing it
cleaned_war_and_peace = war_and_peace.replace("\n", " ")
print(cleaned_war_and_peace)
```

Now that we've read the file, let's begin to process it. First, we install our packacges-NLTK, spaCy, and Stanza--using pip and import them.


```python
# install the packages, if you haven't already
pip install nltk
pip install spacy
pip install stanza
```

Now that the code is imported, let's split the text into sentences and detect each sentence's language. We'll begin by tokenizing using NLTK.


There are different sentence tokenizers included in the NLTK package. NLTK recommends using the [PunktSentenceTokenizer](https://www.nltk.org/api/nltk.tokenize.html) for a language specified by the user, but if working with multilingual text this may not be the best approach, as you will be applying a tokenization model targeted at one language to all of the languages in your text. For the purposes of this tutorial, we will use the sent_tokenize method built into NLTK without specifying a language.


```python
# first, let's install the 'punkt' resources required to use the tokenizer
import nltk
nltk.download('punkt')

# then we import the sent_tokenize method and apply it to our war_and_peace variable
from nltk.tokenize import sent_tokenize
nltk_sent_tokenized = sent_tokenize(cleaned_war_and_peace)
# if you were going to specify a language, the following syntax would be used: nltk_sent_tokenized = sent_tokenize(war_and_peace, language="russian")
```

The entire text is now accessible as a list of sentences within the variable nltk_sent_tokenized. Let's print three sentences we'll be working with: one entirely in Russian, one entirely in French, and one that is in both languages:


```python
# Russian only
rus_sent = nltk_sent_tokenized[5]
print('Russian: ' + rus_sent)

# French only
fre_sent = nltk_sent_tokenized[2]
print('French: ' + fre_sent)

# both French and Russian
multi_sent = nltk_sent_tokenized[4]
print('Multilang: ' + multi_sent)
```

Now let's perform the same sentence tokenization using spaCy, grabbing the same sample of three sentences.


```python
import spacy
# downloading our multilingual sentence tokenizer
python -m spacy download xx_sent_ud_sm

# loading the multilingual sentence tokenizer we just downloaded
nlp = spacy.load("xx_sent_ud_sm")
# applying the spaCy model to our text variable
doc = nlp(cleaned_war_and_peace)

# assigning the tokenized sentences to a list so it's easier for us to manipulate them later
spacy_sentences = list(doc.sents)

print(spacy_sentences)
```

Let's assign our sentences to variables, like we did with NLTK. spaCy returns its sentences not as strings, but as spaCy tokens. In order to print them as we did with the NLTK sentences above, we'll need to convert them to strings in order to concatenate them with the prefix identifying their language:


```python
# Russian only
spacy_rus_sent = str(spacy_sentences[5])
print('Russian: ' + spacy_rus_sent)

# French only
spacy_fre_sent = str(spacy_sentences[2])
print('French: ' + spacy_fre_sent)

# both French and Russian
spacy_multi_sent = str(spacy_sentences[4])
print('Multilang: ' + spacy_multi_sent)
```

We can see above that both models tokenized the sentences in the same way, as the NLTK and spaCy indices match to the same sentences from the text. Now, let's perform the same operation with Stanza, using its built-in multilingual pipeline.


```python
import stanza

from stanza.pipeline.multilingual import MultilingualPipeline

nlp = MultilingualPipeline(processors='tokenize')
doc = nlp(cleaned_war_and_peace)
# printing all sentences
print([sentence.text for sentence in doc.sentences])
```

Now, let's find the same sentences we did with NLTK and spaCy above. First, let's add the sentence tokens to a list, converting them to strings. This makes it easier for us to look at specific sentences by their indices.


```python
stanza_sentences = []
for sentence in doc.sentences:
  stanza_sentences.append(sentence.text)

# Russian only
stanza_rus_sent = str(stanza_sentences[5])
print('Russian: ' + stanza_rus_sent)

# French only
stanza_fre_sent = str(stanza_sentences[2])
print('French: ' + stanza_fre_sent)

# both French and Russian
stanza_multi_sent = str(stanza_sentences[4])
print('Multilang: ' + stanza_multi_sent)
```
### Identifying Languages 

Now that we have three sentences to use as examples, we can begin to perform our analysis of each one. First, let's detect the language of each sentence computationally, starting with the monolingual examples.

NLTK has the TextCat module for language identification using the TextCat algorithm; let's try that on our sentences below.


```python
# downloading an NLTK corpus reader required by the TextCat module
nltk.download('crubadan')

# loading the TextCat package and applying it to each of our sentences
tcat = nltk.classify.textcat.TextCat()
rus_estimate = tcat.guess_language(rus_sent)
fre_estimate = tcat.guess_language(fre_sent)
multi_estimate = tcat.guess_language(multi_sent)

# printing the results
print(rus_estimate)
print(fre_estimate)
print(multi_estimate)
```

As we can see, TextCat correctly identified the Russian and French sentences. Since it can't output more than one language per sentence, it guessed Russian for our multilingual sentence. We'll examine other ways to handle language detection for multilingual sentences after we perform our sentence classification using spaCy and Stanza.


```python
# now, let's look at language classification for sentences using spaCy
# first, we install the spacy_langdetect package from the Python Package Index, then we import it
# and use it to detect our languages

pip install spacy_langdetect

from spacy.language import Language
from spacy_langdetect import LanguageDetector

# setting up our language detector to work with spaCy
def get_lang_detector(nlp, name):
    return LanguageDetector()

Language.factory("language_detector", func=get_lang_detector)
nlp.add_pipe('language_detector', last=True)

# running the language detection on each sentence and printing the results
rus_doc = nlp(spacy_rus_sent)
print(rus_doc._.language)

fre_doc = nlp(spacy_fre_sent)
print(fre_doc._.language)

multi_doc = nlp(spacy_multi_sent)
print(multi_doc._.language)
```

We got similar, expected results with spaCy; note the confidence printed after the language guess is far lower for the multilingual sentence given that it contains more than one language. Now let's try Stanza, which has a built-in language identifier.


```python
# importing our models required for language detection
from stanza.models.common.doc import Document
from stanza.pipeline.core import Pipeline

# setting up our pipeline
nlp = Pipeline(lang="multilingual", processors="langid")

# specifying which sentences to run the detection on, then running the detection code
docs = [stanza_rus_sent, stanza_fre_sent, stanza_multi_sent]
docs = [Document([], text=text) for text in docs]
nlp(docs)

# printing the text of each sentence alongside the language estimates
print("\n".join(f"{doc.text}\t{doc.lang}" for doc in docs))
```

We can see that Stanza classified the final sentence as French, deviating from the other models.

For multilingual sentences, classifying both languages within the same sentence is not a simple problem to solve, and requires more granular analysis than a sentence-by-sentence approach. One method to detect all of the languages contained in a single sentence, for example, would be to break the sentence into its component words and then try to detect the language of each word--which will have questionable accuracy, given that we are only looking at one word at a time--and then group consecutive words of the same language within the string into new strings consisting only of one language. For this example, we can also detect non-Roman script and split the string into its component languages that way. Here is a simple implementation:


```python
# first, we split the sentence into its component words
from nltk.tokenize import wordpunct_tokenize
tokenized_sent = wordpunct_tokenize(multi_sent)

# then, we check if each word contains Cyrillic characters, and split the string into two strings of Cyrillic and non-Cyrillic script
# for simplicity's sake, we'll omit any punctuation in this example

# importing the regex package so we can use a regular expression
import regex
# importing the string package to detect punctuation
from string import punctuation

# setting empty lists we will later populate with our words
cyrillic_words = []
latin_words = []

# iterating through our sentence and using regex to detect Cyrillic characters
# if Cyrillic is found, we append the word to our cyrillic_words list; otherwise, we append the word to the Latin list
# if a tokenized word is only punctuation, we continue without appending
for word in tokenized_sent:
  if regex.search(r'\p{IsCyrillic}', word):
    cyrillic_words.append(word)
  else:
    if word in punctuation:
       continue
    else:
        latin_words.append(word)


print(cyrillic_words)
print(latin_words)

# we can then join these lists into strings to run our language detection code
cyrillic_only_list = ' '.join(cyrillic_words)
latin_only_list = ' '.join(latin_words)

print(cyrillic_only_list)
print(latin_only_list)

# now we use TextCat again to detect their languages
tcat = nltk.classify.textcat.TextCat()
multi_estimate_1 = tcat.guess_language(cyrillic_only_list)
multi_estimate_2 = tcat.guess_language(latin_only_list)

# printing our estimates
print(multi_estimate_1)
print(multi_estimate_2)

```

### Part of Speech Tagging

Now, let's perform part-of-speech tagging for our sentences using spaCy and Stanza.

NLTK does not support POS tagging on languages other than English out-of-the-box, but you can train your own model using a corpus to tag languages other than English. Documentation on the tagger, and how to train your own, can be [found here](https://www.nltk.org/book/ch05.html#sec-n-gram-tagging).

Tagging our sentences with spaCy is very straightforward. Since we know we're working with Russian and French, we can download the appropriate spaCy corpora and use them to get the proper POS tags for the words in our sentences. The syntax remains the same regardless of which language model we use.


```python
# French first

# downloading our French and Russian corpora from spaCy
python -m spacy download fr_core_news_sm
python -m spacy download ru_core_news_sm


# loading the corpus
nlp = spacy.load("fr_core_news_sm")
doc = nlp(spacy_fre_sent)
# printing the text of each word and its POS tag
for token in doc:
    print(token.text, token.pos_)

# And doing the same with our Russian sentence
nlp = spacy.load("ru_core_news_sm")
doc = nlp(spacy_rus_sent)
print(doc.text)
for token in doc:
    print(token.text, token.pos_)

```

For multilingual text, we can use the words we generated earlier to tag each language separately, then join the words back into a complete sentence again.

```python
# We split our sentence into Russian and French words as before, only this time we preserve the punctuation
# by appending any punctuation we encounter to the last list we appended to, preserving the proper placement
# of our punctuation (e.g., it will append a period to the word it should follow).

cyrillic_words = []
latin_words = []
# initializing a blank string to keep track of the last list we appended to
last_appended_list = ''

for word in tokenized_sent:
  if regex.search(r'\p{IsCyrillic}', word):
    cyrillic_words.append(word)
    # updating our string to track the list we appended a word to
    last_appended_list = 'cyr'
  else:
    # handling punctuation
    if word in punctuation:
        if last_appended_list == 'cyr':
            cyrillic_words.append(word)
        elif last_appended_list == 'lat':
            latin_words.append(word)
    else:
        latin_words.append(word)
        last_appended_list = 'lat'

print(cyrillic_words)
print(latin_words)

# We can then join these lists into strings to run our language detection on them.
# We'll use a regular expression to remove the extra whitespace before each punctuation mark
# that was created when we tokenized the sentence into words. This will preserve the punctuation
# as it was present in the original sentence.

cyrillic_only_list = ' '.join(cyrillic_words)
latin_only_list = ' '.join(latin_words)

cyr_no_extra_space = regex.sub(r'\s([?.!"](?:\s|$))', r'\1', cyrillic_only_list)
lat_no_extra_space = regex.sub(r'\s([?.!"](?:\s|$))', r'\1', latin_only_list)

print(cyr_no_extra_space)
print(lat_no_extra_space)

# Finally, we can tag each list of words using the appropriate language model.
# loading the corpus
nlp = spacy.load("fr_core_news_sm")
doc = nlp(lat_no_extra_space)
# printing the text of each word and its POS tag
for token in doc:
    print(token.text, token.pos_)

# And doing the same with our Russian sentence
nlp = spacy.load("ru_core_news_sm")
doc = nlp(cyr_no_extra_space)
print(doc.text)
for token in doc:
    print(token.text, token.pos_)

```

Now, let's perform POS tagging using Stanza. The syntax is quite straightforward:


```python
# French
# loading our pipeline and applying it to our sentence
nlp = stanza.Pipeline(lang='fr', processors='tokenize,mwt,pos')
doc = nlp(stanza_fre_sent)

# printing our words and POS tags
print(*[f'word: {word.text}\tupos: {word.upos}' for sent in doc.sentences for word in sent.words], sep='\n')

# Russian
nlp = stanza.Pipeline(lang='ru', processors='tokenize,pos')
doc = nlp(stanza_rus_sent)
print(*[f'word: {word.text}\tupos: {word.upos}' for sent in doc.sentences for word in sent.words], sep='\n')


```

We'll now take a simpler approach for the multilingual analysis than we did with spaCy, as Stanza's multilingual pipeline allows us to return POS tags with similar syntax to the examples above.

```python

# running the multilingual pipeline on our French, Russian, and multilingual sentences simultaneously
nlp = MultilingualPipeline(processors='tokenize,pos')
docs = [stanza_rus_sent, stanza_fre_sent, stanza_multi_sent]
nlp(docs)
print(*[f'word: {word.text}\tupos: {word.upos}' for sent in doc.sentences for word in sent.words], sep='\n')
```

### Lemmatization

Finally, let's perform lemmatization on our sentences using spaCy and Stanza (NLTK does not provide out-of-the-box lemmatization for non-English languages). Lemmatization is the process of grouping together the inflected forms of a word so they can be analysed as a single item, identified by the word's lemma, or dictionary form.

spaCy does not have a single multingual lemmatization corpus, so we'll have to run separate models on our Russian and French text and split our multilingual sentence into its component parts again.

```python
# spaCy
# More info, including supported languages, can be found here: https://spacy.io/api/lemmatizer


# French
fre_nlp = spacy.load("fr_core_news_sm")
doc = nlp(spacy_fre_sent)

for token in doc:
    print(token, token.lemma_)

# Russian
nlp = spacy.load("ru_core_news_sm")
doc = nlp(spacy_rus_sent)

for token in doc:
    print(token, token.lemma_)

# Multilingual
# We'll run each model on the chunks of text we split apart earlier.

# Russian
doc = nlp(cyr_no_extra_space)

for token in doc:
    print(token, token.lemma_)

# French
doc = nlp(lat_no_extra_space)

for token in doc:
    print(token, token.lemma_)



```

And now in Stanza:

```python
# this syntax is very similar to the POS tagging with the multilingual pipeline we used earlier

# adding the 'lemma' processor to the pipeline and running it on our sentences
nlp = MultilingualPipeline(processors='tokenize,lemma')
docs = [stanza_rus_sent, stanza_fre_sent, stanza_multi_sent]
nlped_docs = nlp(docs)
# iterating through each sentence and printing the lemmas
for doc in nlped_docs:
  lemmas = [word.lemma for t in doc.iter_tokens() for word in t.words]
  print(lemmas)
```

## Conclusion

You now have a basic knowledge of each package that can help guide your use of the packages for your personal projects. You also have a basic understanding of how to approach non-English text using computational tools, and some strategies for working with multilingual text that will help you develop methodologies and strategies for applying your own workflows to analyzing other multilingual texts.

## Suggested Readings

**Related Programming Historian Lessons:**

The following lessons can help with other important aspects of working with textual data that can be applied to non-English and multilingual texts.

[Corpus Analysis with spaCy](https://programminghistorian.org/en/lessons/corpus-analysis-with-spacy)

This lesson is an in-depth look at analyzing a corpus using spaCy, and goes into details of spaCy’s capabilities and syntax we didn’t have time for in this lesson. This is a highly recommended read if you plan to use spaCy more in-depth for your work.

[Normalizing Textual Data with Python](https://programminghistorian.org/en/lessons/normalizing-data)

This lesson explains various methods of data normalization using Python, and will be very useful for anyone who needs a primer on how to prepare their textual data for computational analysis.

**Other resources about multilingual text analysis and DH:**

[Multilingual Digital Humanities](https://doi.org/10.4324/9781003393696)

A recently published book covering various topics and projects in the field of multilingual digital humanities, featuring a broad range of authors and geared toward an international audience. (Full disclosure: I have a chapter in this).

[multilingualdh.org](https://multilingualdh.org/en/)

The homepage for the Multilingual DH group, a “loosely-organized international network of scholars using digital humanities tools and methods on languages other than English.” The group’s [GitHub repository](https://github.com/multilingual-dh) has helpful resources, as well, including [this bibliography](https://github.com/multilingual-dh/multilingual-dh-bibliography) and [this list of tools for multilingual NLP](https://github.com/multilingual-dh/nlp-resources).
