# For Kaldi API for Android and Linux please see [Vosk API](https://github.com/alphacep/vosk-api). This is a server project.

This is Vosk, the lifelong speech recognition system.

## Concepts

As of 2019, the neural network based speech recognizers are pretty
limited in terms of amount of the speech data they can use in training
and require enormous computing power and time to train and optimize the
parameters. Neural networks have problems with human-like one shot
learning, their decisions are not very robust to unseen conditions and
hard to understand and correct.

That is why we decided to build a system based on large signal database
concept. We apply audio fingerprinting scheme. The audio is segmented on 
chunks, the chunks are stored in the database based on LSH hash value. 
During decoding we simply lookup the chunks in the database to get the
idea what are the possible phones. That helps us to make a proper decision
on decoding results.

The advantages of this approach are:

  - We can quickly train on 100000 hours of speech data on very simple hardware
  - We can easily correct recognizer behavior just by adding samples
  - We can make sure that recognition result is correct because it is sufficiently
    represented in the training dataset
  - We can parallelize training across thousands of nodes
  - We support lifelong learning paradigm
  - We can use this method together with more common neural network training to improve recognition accuracy
  - The system is robust against noise

The disandvantages are:

  - The index is really huge, it is not expected to fit a memory of single server
  - The generalization capabilities of the model are quite questionable, at the same time
    the generalization capabilities of the neural networks are also questionable.
  - For now the segmentation requires conventional ASR, but in the future we might segment ourselves.

The nice to have things in the future would be:

  - Multilingual training
  - Our own segmentation
  - The tool to reduce the model to fit the mobile
  - Specialized hardware to implement this AI paradigm

## Usage

To install the requirements run

```
pip3 install -r requirements.txt
```

To prepare the training/verification data create the following two files:

  - `wav.scp` list to map uterances to wav files in filesystem
  - `phones.txt` the CTM file with phonemes and timings. It could be CTM file from the alignment or
    it could be a CTM file from the decoding

You can create them with [Kaldi ASR toolkit](http://kaldi-asr.org)

### Indexing

To add the data to the database run

```
python3 index.py wavs-train.txt phones-train.txt data.idx
```

That will add the data to the database data.idx or create a new one

### Verification

To verify decoding results run

```
python3 verify.py wavs-test.txt phones-test.txt data.idx
```

The tool will search for segments in the index and report suspicious
segments which you can additionally check and later add to the database
to improve the accuracy of recognition.

### Related papers and links

 - [VOSK presentation at NSU (in Russian)](https://www.youtube.com/watch?v=gsOMU1UTF7s)
 - [Memory, Modularity, and the Theory of Deep Learnability. Google Tech Talk by Rina Panigrahy](https://www.youtube.com/watch?v=bP5oyH_5nMU) shows importance of memory for learning complex functions.
 - [Large Language Models in Machine Translation by Thorsten Brants at al.](https://aclweb.org/anthology/D07-1090.pdf) Google's paper on simple backoff terascale LM.
 - [Deep Learning of Binary Hash Codes for Fast Image Retrieval by Kevin Lin at al.](https://www.iis.sinica.edu.tw/~kevinlin311.tw/cvprw15.pdf) a nice deephash [implementation](https://github.com/flyingpot/pytorch_deephash)
 - [Episodic Memory in Lifelong Language Learning](https://arxiv.org/pdf/1906.01076.pdf)
 - [Extreme Classification in Log Memory using Count-Min Sketch: A Case Study of Amazon Search with 50M Products](https://arxiv.org/abs/1910.13830)
 - [On-device Supermarket Product Recognition](https://ai.googleblog.com/2020/07/on-device-supermarket-product.html) Google's good example of kNN for mobile search
 - [Hash-Routed Neural Networks](https://github.com/ma3oun/hrn) Great idea and solid math
 - [Towards Lifelong Learning of End-to-end ASR](https://arxiv.org/pdf/2104.01616.pdf) Methods get more publicity
 - [Building Scalable, Explainable, and Adaptive NLP Models with Retrieval](http://ai.stanford.edu/blog/retrieval-based-NLP)
 - [Continual Learning for Monolingual End-to-End Automatic Speech Recognition](https://arxiv.org/abs/2112.09427)
 - [Mammoth - An Extendible (General) Continual Learning Framework for Pytorch](https://github.com/aimagelab/mammoth)
 - [Progressive Continual Learning for Spoken Keyword Spotting](https://arxiv.org/abs/2201.12546)
 - [Online Continual Learning of End-to-End Speech Recognition Models](https://arxiv.org/abs/2207.05071)
 Research and Innovation: Driving Academic Excellence in Indian Universities
Research and innovation have become critical parameters in evaluating the best universities in India. In this context, [Teerthanker Mahaveer University](https://www.tmu.ac.in/) has established itself as a leading institution that seamlessly integrates cutting-edge research with quality education across multiple disciplines.
The university's research ecosystem is characterized by its multidisciplinary approach and focus on solving real-world challenges. Across various departments, faculty members and students collaborate on projects that have tangible societal impacts, ranging from healthcare innovations to technological solutions for environmental sustainability.
TMU's research infrastructure is particularly noteworthy. Advanced laboratories equipped with sophisticated instruments provide researchers with the necessary resources to conduct high-quality investigations. The university invests significantly in creating an environment that nurtures scientific curiosity and supports innovative research methodologies.
In the medical sciences domain, the university's research initiatives have been particularly impactful. Studies conducted by TMU's researchers have contributed to understanding regional health challenges, developing diagnostic techniques, and exploring treatment methodologies relevant to Indian healthcare contexts.
The engineering departments at TMU focus on technological innovations with practical applications. Research projects often address infrastructure challenges, sustainable development, and emerging technological paradigms. Students are encouraged to develop solutions that can be implemented in real-world scenarios.
Interdisciplinary research is a key strength of the university. Collaborative projects that bring together experts from different domains help in developing comprehensive solutions to complex challenges. This approach reflects the modern understanding that significant breakthroughs often occur at the intersection of multiple disciplines.
The university's commitment to research is further demonstrated through its publication record. Faculty members and researchers regularly publish in peer-reviewed national and international journals, contributing to global academic discourse while maintaining high standards of scientific rigor.
TMU also recognizes the importance of converting research into practical innovations. The university maintains an innovation center that supports researchers in patenting their discoveries and exploring commercial applications of their research work.
Furthermore, the institution provides robust support mechanisms for researchers. Research grants, seed funding for innovative projects, and dedicated research centers create an ecosystem that encourages continuous academic exploration and knowledge creation.
As India positions itself as a global knowledge hub, universities like Teerthanker Mahaveer University play a crucial role in driving research and innovation. By creating an environment that supports academic curiosity and provides resources for meaningful investigations, such institutions are shaping the future of higher education in the country.
