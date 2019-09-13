# German ELMo Model
This is a German [ELMo deep contextualized word representation](https://allennlp.org/elmo). It is trained on a special [German Wikipedia Text Corpus](https://github.com/t-systems-on-site-services-gmbh/german-wikipedia-text-corpus).

ELMo is a deep contextualized word representation that models both 
1. complex characteristics of word use (e.g., syntax and semantics)
2. how these uses vary across linguistic contexts (i.e., to model polysemy)

These word vectors are learned functions of the internal states of a deep bidirectional language model (biLM), which is pre-trained on a large text corpus. They can easily be added to existing models and significantly improve the state of the art across a broad range of challenging NLP problems, including question answering, textual entailment and sentiment analysis. More about these "Deep contextualized word representations" can be found here: https://arxiv.org/abs/1802.05365

This pre-trained model can be used with [bilm-tf](https://github.com/allenai/bilm-tf) (the TensorFlow implementation of ELMo). Before usage it can (should) be fine-tuned with your domain-specific data (see below). Unlike most other ELMo models, we also provide the checkpoint files which enable you to do the fine-tuning.

## How this ELMo Model has been trained
This ELMo model has been trained by using the [TensorFlow version of bilm](https://github.com/allenai/bilm-tf). It was trained on the special [German Wikipedia Text Corpus](https://github.com/t-systems-on-site-services-gmbh/german-wikipedia-text-corpus).

The advantage of this text corpus is that it does not only contain the article space of the wiki, but also the comments for a larger text corpus and a more sloppy language. This should improve the quality of downstream tasks when you process conversations like mails, chats, tweets or support tickets.

The text corpus has been split to train set and to test set. The train set had 5.8GB of text data with 962,868,231 tokens. The test set had the size of 87MB.

The vocabulary file was generated by [this script](https://github.com/PhilipMay/de-wiki-text-corpus-tools/blob/master/vocab_file_writer.py). It limits the size of the vocabulary to the most frequent 800,000 tokens. This limitation is necessary to avoid out-of-memory errors during training time.

Now training was done like this:
1. Start training with [train_elmo.py](https://github.com/allenai/bilm-tf/blob/master/bin/train_elmo.py)
2. calculate perplexity on test set with [run_test.py](https://github.com/allenai/bilm-tf/blob/master/bin/run_test.py)
3. continue training with [restart.py](https://github.com/allenai/bilm-tf/blob/master/bin/restart.py)
4. loop back to step 2 until the bilm starts to overfit

This has been done for 15 epochs. Epoch 15 was overfitting; so the best perplexity was on epoch 14 with 40.17 (see graphic below).

![Validation perplexity](https://raw.githubusercontent.com/t-systems-on-site-services-gmbh/german-elmo-model/master/perplexity-german-bilm.png "Validation perplexity")

Training time on a single GeForce GTX 1080 Ti was about 68 hours per epoch and test time about 2,5 hours. So training this ELMo model took more than 44 days of computation time.

## How to use this ELMo Model
The general usage options of an ELMo model are described here: https://github.com/allenai/bilm-tf

For different concrete usage options see here:
- [usage_cached.py](https://github.com/allenai/bilm-tf/blob/master/usage_cached.py)
- [usage_character.py](https://github.com/allenai/bilm-tf/blob/master/usage_character.py)
- [usage_token.py](https://github.com/allenai/bilm-tf/blob/master/usage_token.py)
- [`dump_bilm_embeddings`](https://github.com/allenai/bilm-tf/blob/master/bilm/model.py#L643)

For fine-tuning see here: [How to do fine tune a model on additional unlabeled data?](https://github.com/allenai/bilm-tf#how-to-do-fine-tune-a-model-on-additional-unlabeled-data)

### Important `n_characters` option
The `n_characters` option has to be hanged between fine tuning and model usage. For fine tuning alwasy set it to `261`. For ELMo model usage set it to `262`. The `n_characters` is set in the `options.json` file. Also see here: https://github.com/allenai/bilm-tf#whats-the-deal-with-n_characters-and-padding

## Content of the Tarball
These files are needed to use the ELMo model:
- vocab-train.txt - vocabulary file
- weights.hdf5 - weights from the trained biLM in hdf5 format
- options.json - options file which describes the hyperparameters of the biLM

These files are part of the TensorFlow checkpoint mechanism. It can save and restore TensorFlow models. This is needed to do fine-tuning (see below):
- checkpoint
- model.ckpt-5265680.index
- model.ckpt-5265680.meta
- model.ckpt-5265680.data-00000-of-00001

## Download
- [bilm-output-result.tgz.part-00](https://github.com/t-systems-on-site-services-gmbh/german-elmo-model/releases/download/files_1/bilm-output-result.tgz.part-00)
  - MD5: 755379908885a6fa6fb5b68e4806eae0
  - SHA1: 39b16d8eaeaa6ff8c161d092fd223704cc3963e6
- [bilm-output-result.tgz.part-01](https://github.com/t-systems-on-site-services-gmbh/german-elmo-model/releases/download/files_1/bilm-output-result.tgz.part-01)
  - MD5: 0a6544017c64cda91a2b209ecbdc5f2c
  - SHA1: 4647c43037b01a67e6c206c9748c5e5b8ab936f8
- [bilm-output-result.tgz.part-02](https://github.com/t-systems-on-site-services-gmbh/german-elmo-model/releases/download/files_1/bilm-output-result.tgz.part-02)
  - MD5: f316774e8bd12d078bd5e869c64be14b
  - SHA1: 10530cb852cf26f6340c2b4d1cfbeb882f44692b
- [bilm-output-result.tgz.part-03](https://github.com/t-systems-on-site-services-gmbh/german-elmo-model/releases/download/files_1/bilm-output-result.tgz.part-03)
  - MD5: ae44a2d3cf95795edacba690b77a4e7b
  - SHA1: c54bcdc27d769e2c5a077d7ab342ee34dd1f3ec2
- bilm-output-result.tgz
  - MD5: 2aa41bbc32d42eae052a7ddf37909443
  - SHA1: 9af7efbdd480a919c45e11188625d59a5943f9b9

## Download Options and Weights Files only
- [options.json](https://github.com/t-systems-on-site-services-gmbh/german-elmo-model/releases/download/files_1/options.json)
  - MD5: da99676f2fe2838ff2663a6d9bad97dd
  - SHA1: 04afb8bc77acf5e65633ced2efc1e91d05332e89
- [weights.hdf5](https://github.com/t-systems-on-site-services-gmbh/german-elmo-model/releases/download/files_1/weights.hdf5)
  - MD5: f3ab3a68caae6d42bac47c230b3a65bf
  - SHA1: 9084adab3eefaed4f07da7331217fcb873417add

## Unpack
Using these commands you can unpack the files (Linux and macOS):
```
cat bilm-output-result.tgz.part-* > bilm-output-result.tgz
tar xvfz bilm-output-result.tgz
```
