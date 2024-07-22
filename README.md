#Project Introduction

This project has built an anonymization system, which mainly consists of two parts. The first part is the identification of personal identity information (PII),
The second part is to pseudonymize personal identity information and ensure that the processed data still has a certain level of usability.

[TOC]



#Anonymization system

##Identification of Personal Identity Information (PII)

This section mainly compares the performance of three methods in PII recognition tasks, namely using the named entity recognition (NER) module of the existing natural language library Spacey,
The existing framework Flair's NER module and the use of Longformer model for PII recognition. In the experiment,
We conducted experiments on three methods using a text dataset (The Text Anonymization Benchmark) designed to detect the quality of anonymization,
Compared their performance in PII recognition tasks.

#### 1.1 Spacy

Spacey is a Python library for natural language processing that provides some tools for text processing, including a Named Entity Recognition (NER) module.
In the experiment, we used Spacey's NER module to identify PII, and then compared it with the PII information annotated in the dataset to calculate relevant indicators.
Due to the fact that Spacey's NER module can only recognize some basic entity types, such as person names, place names, organization names, etc., we only considered these entity types in the experiment.
PERSON: Person Name LOC: Place Name ORG: Organization Name
For CODE: PII information such as personal ID number, license plate number and mobile phone number, Spacy cannot identify it. In the experiment, we used regularization method to identify as much as possible.
For details, please refer to checkpoint/Pacy_methodpy

#### 1.2 Flair

Flair is a Python library for natural language processing that provides tools for text processing, including named entity recognition (NER) modules.
In the experiment, we used Flair's NER module to identify PII, and then compared it with the PII information annotated in the dataset to calculate relevant indicators.
Although Flair's NER module can recognize more entity types, such as person names, place names, organization names, dates, times, etc., other information such as dates and times is not very helpful for identifying personal identity information,
So we will only consider the entity type:
PERSON: Person Name LOC: Place Name ORG: Organization Name
For CODE: PII information such as personal ID number, license plate number and mobile phone number, Spacy cannot identify it. In the experiment, we used regularization method to identify as much as possible.
For more details, please refer to checkpoint/Filer_methodpy

#### 1.3 Longformer

Longformer is an efficient model for processing long texts, published in a paper by Allen Institute for Artificial Intelligence (AI2) on April 10, 2020. It is a Transformer variant customized for long texts, aimed at solving the memory bottleneck problem that traditional Transformer models face when processing long texts. Longformer proposed a Self Attention mechanism where the spatiotemporal complexity is linearly related to the length of the text sequence, enabling the model to model long documents with lower spatiotemporal complexity. Longformer has shown excellent performance in high difficulty reading comprehension tasks, such as TriviaQA, Wikihop, and HotpotQA. In the experiment, we used the Longformer model to identify PII, and then compared it with the PII information annotated in the dataset to calculate relevant indicators.

For details, please refer to checkpoint/Longformer_medol.xy (creating a model) and checkpoint/data_manding.py (processing and aligning data)
Checkpoint/data_manipulation. py (handling anonymized text) Checkpoint/train_madel. py (training model, printing prediction results)
See Myreadme.md for details

####1.4 Modified Longformer

Due to the fact that the original longformer model can only output the starting and ending positions of entities, and cannot output the type of entity, we have made modifications to the longformer model,
Enable it to output the type of entity. Specifically, in the data preprocessing stage, the annotation of entity types in the anonymized dataset was added to the input of the model, and the output layer of the model was modified.
Please refer to Myreadme.md for details, Longformer_medol.py，data_handing.py，data_manipulation.py，train_model.py

####1.5 Evaluation indicators

Since the evaluation script only requires a standard corpus and a predicted anonymized result file, it can be directly used for evaluation. Due to script requirements, the output of the three methods mentioned above stored in the checkpoint folder only includes the tuple of the starting and ending positions of the entity that needs to be anonymized in the text, without including its entity type and entity text information. Please refer to Myreadme.md for details.

##2. Pseudonymize personal identification information

This section mainly processes the results of PII recognition, replacing PII information with forged information to protect user privacy.
Based on the comparison of the first part, it was found that the Longformer model trained on the Text Anonymization Benchmark had the best performance in PII recognition tasks. Therefore, we chose to use the Longformer model for PII recognition and replace the recognized information with two methods.

####2.1 Replace PII information with randomly generated pseudonyms

Here, a database faker that can randomly generate pseudonym information is used. By calling the corresponding faker function on the entity type labels recognized by the longformer model,
Replace PII information with generated pseudonym information under the corresponding entity type label.
For details, please refer to run-longmodel.py

####2.2 Using Rotation Method to Replace PII Information

Here, the entities of the same type recognized by the longformer model are grouped, and then each group of entities is rotated and replaced, that is, one type of entity is replaced with another entity under the same type group.
For details, please refer to run-longmodel.py

####2.3 Evaluation indicators for pseudonymization experiments

Longformer model recognizes PII and implements pseudonymization using two methods. We have constructed two systems
Use the above anonymization system to anonymize the IMDB dataset, and then use Myclassifier. py to classify the three datasets, namely the original data, the faker pseudonymized data, and the rotated pseudonymized data.
Compare the precision, recall, f1 score, accuracy, and other metrics of the same BERT model on three different datasets to evaluate the effectiveness of the anonymization system. Please refer to MYclassifier. py for details

Using evaluation.py, since the evaluation script only requires a standard corpus and a prediction anonymized result file, it can be directly used for evaluation.
Due to script requirements, the output of the three methods mentioned above stored in the checkpoint folder only includes the tuple of the starting and ending positions of the entity that needs to be anonymized in the text, without including its entity type and entity text information. Please refer to Myreadme.md for details.
