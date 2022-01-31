In this repo, I will share what I know about training a Japanese-English translation model.

**How to effectively search for research papers:**
The field of applying deep learning technologies to machine translation is known as **Neural Machine Translation** or **NMT**. This is important to know because if you have a vague idea of what you want to do, you can **search Google** in this pattern: "**Your idea + NMT**". For example, if you are interested in translation that keep context in mind, you can search for "contextual NMT". 

**An overview of modern NMT:**
Current **state of the art or SOTA** architecture for NMT is called **Transformer**. This type of deep learning model came out in 2017 thanks to **Vaswani** with a very catchy title: "[Attention is all you need](https://arxiv.org/abs/1706.03762 "Attention is all you need")". Since then, there has been many variants of the original transformer architecture but Vaswani Transformer remained to be very effective and readily available.

**What is Transformer?**
The **purpose** of a Transformer model is to **translate a sequence to another sequence (seq2seq)**. In the context of NMT, this means translating a Japanese sentence to an English sentence.

**The building block of each sequence** in general is known as "**token**". The simplest form of token in the context of NMT is a word. For example, "Hello World" has two tokens "Hello" and "World"

**Most common NMT frameworks: **
**HuggingFace**: This is the most comprehensive and beginner-friendly framework for NLP in general, including translation. Personally, I haven't tried it but the tutorials and reputation seemed to be easy to follow. 

**Fairseq**: This is also a general NLP framework but made by Facebook. Many state of the art models are implemented in Fairseq and the inference/output speed is very fast. 

**OpenNMT:** A fairly popular NMT specific framework. Some of it modules can also be used with Fairseq and HuggingFace.

**Training data for NMT Transformer:**
For training a Japanese to English model, you need to prepare a list of bilingual sentence pairs. As an example, a txt file of 5 million Japanese lines with a corresponding txt file of 5 million English lines.

**General resources for NMT data:**
[Opus Corpus](https://opus.nlpl.eu/index.php "Opus Corpus"): The most abundant resources for any language pairs. Assembled by university of Helsinky, any common NMT dataset that you know or may not know is here. Format is in matching line by line txt files.

[OpenSubtitle2018](https://opus.nlpl.eu/OpenSubtitles-v2018.php "OpenSubtitle2018"): This is a movie subtitle dataset for all kind of language pairs, suitable for conversational translation.

[CCMatrix](https://opus.nlpl.eu/CCMatrix.php "CCMatrix"): The largest bilingual pair of data in any language you can think of. Sentences are mined and aligned from Wikipeadia by the Facebook Research Team. Dataset can be found on Opus Corpus.

[CCAligned](https://opus.nlpl.eu/CCAligned.php "CCAligned"): A big resource of general web scraping dataset from Facebook. Alignment is better than CCMatrix although content is more limited.

**Japanese-English specific resources:**
[Neubig dataset hub](http://www.phontron.com/japanese-translation-data.php "Neubig dataset hub"):  A list of interesting NMT resources for JP-EN language pair assembled by professor Graham Neubig

[JparaCrawl](http://www.kecl.ntt.co.jp/icl/lirg/jparacrawl/ "JparaCrawl"): a high quality and big dataset of sentences mined from bilingual websites. Also included a model and tutorial for fine-tuning  Credit to the research team from NTT.

[JESC:](https://nlp.stanford.edu/projects/jesc/ "JESC:") The biggest dataset available for Japanese-English subtitles with around 2 million lines but quality is not good in my opinion. Sentences are shuffled, all words are all lowercased, honorific are stickied with names (ie. Nakamurasan). 

[harikc456 Subtitle Dataset](https://github.com/harikc456/anime-subs-mapping "harikc456 Subtitle Dataset"): contained around 500k subtitles lines from anime. Data is in the form of CSV. 

**Common Data Preprocessing Steps:**
The first one is **deduplication**. Remove training data with the same content, keep the unique ones.

The second one is **data shuffling**. Let's say you have a txt file of Japanese lines, randomize the order of these lines. 

Last step is **tokenization**, which I will in more details in the bottom section.

An optional step that I often feel the need to do is **data normalization**. That's means to **keep data in the same format as possible**. For example, it's very common for Japanese text to include square bracket quotation 「」and the english translation will be a mix of keeping the bracket or changing it to normal quotation marks "". In this case, to make it easier for the model to learn, I would edit english data to follow whatever quotation marks in the Japanese side.

**The need for Tokenizer:**
After you have the training data as mentioned above, you **need to tokenize each line** with a tokenizer. **A tokenizer** splits a sequence/line of words into **a list of tokens**. Many NMT models won't accept raw sentences so you need to tokenize your data to finalize the training data. 

**Tokenizer overview:**
The **most common form** of NMT token is **subword**. For example, the word "doing" will be two tokens "do" and "ing". This kind of token is found to be optimal in speed and accuracy compare to word or character tokens. 

The most popular implementation of subword tokenization is [sentencepiece](https://github.com/google/sentencepiece "sentencepiece") (SPM) and [Byte-Pair Encoding](https://github.com/rsennrich/subword-nmt "Byte-Pair Encoding") (BPE). 

Either you will have to manually install and use the above packages or use the bundled implementation of your framework (HuggingFace, Pytorch, etc).
