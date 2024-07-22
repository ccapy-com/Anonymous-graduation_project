###Four files of longformer
These four Python files are an implementation of a machine learning model for identifying and masking personal identity information (such as name, address, etc.). Their main functions are as follows:
data_handling.py
Defined classes and functions for processing training data
Including marking and aligning input text, dividing it into small batches of data, and other operations
Pay attention to alignment operation, I B O inside
data_manipulation.py
Read training, development, and testing datasets in raw JSON format
Preprocess the data and merge the annotation results of different annotators into one annotation
Merge label qusi and direct into MASK, merge NOmask into NOmask
longformer_model.py
Defined a sequence annotation model architecture based on Longformer pre trained model
Added a linear classification layer on the output of Longformer for masking recognition
train_model.py
The main script for loading data, initializing models, and training models
Evaluate model performance on the development set
Make predictions on the test set and save the results as a JSON file
Save the final trained model parameters
Overall, this is an end-to-end personal identification and privacy protection system that can be used to automatically detect and mask sensitive personal information in text. It uses the Longformer model to process long text input.

###Evaluation.py script

This Python script is used to evaluate the performance of a text anonymization system. It can calculate multiple evaluation metrics, including token level, mention level, and entity level accuracy and recall, and provide detailed error analysis information.
Usage:
python evaluation.py <gold_standard_file> <masked_output_file> [options]
Required parameters:
<gold_standandard_file>JSON file path containing standard comments
<Masked_output FILE>One or more JSON file paths containing the system output mask text range

Optional parameters:
--Use_cert uses BERT language model to calculate the information weight of each identifier
--only_docs <doc_id1> <doc_id2>...
Only evaluate the specified document ID
--Verbose prints out incorrectly masked mentions for easy error analysis

Output:
This script will print out the following evaluation metrics:
-Token level recall rate (overall and categorized by entity type)
-Recall rate at the Mention level
-Entity level recall rate (direct identifier and quasi identifier)
-Token level precision (uniform weighting and BERT weighting)
-Precision at the Mention level (uniform weighting and BERT weighting)

matters needing attention:
1. This script assumes that the input JSON file format conforms to a specific standard.
If using the -- use_dert option, you will need to download the BERT model (approximately 500MB).
3. A separate evaluation result will be output for each provided masked_Output.

This evaluation script is based on text anonymization annotation guidelines and standard corpora. For detailed information, please refer to the code comments in the file.

###Modify the longformer model to suit subsequent downstream tasks
Modify the longformer model to a model that can recognize entity labels
change1 data_manipulation
Extend the label with PERSONMASK, CODEMASK，LOCMASK，ORGMASK，DEMMASK，DATETIMEMASK，QUANTITYMASK，MISCMASK
Change 2: Modify train_comodel. py
Label Set (labels=['PERSONMASK ',' CODEMASK ',' LOCMASK ',' ORGMASK ',
'DEMMASK', 'DATETIMEMASK', 'QUANTITYMASK', 'MISCMASK'])
Change 3: Modify the alignment function at the bottom of data_mandling
if anno['label'] != ' NO_MASK':
annotation_token_ix_set = (
set()
) 
Change 4: Modify the BIO recognition function of try in the train model
Change 5: Cross entropy in the training model