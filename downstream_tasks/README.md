
## Explanation of Input to NER model + labels
The model is given a sentence to predict a tag for as the context for a sentence. The labels include BIO tags for treatment, problem, and test, and “X”, which is a special “continuation label” that denotes the continuation of the first label (for example with wordpiece tokenization, ti can split a token into several tokens. If the original token had some label A, then the label of the modified token would be A, X. 

## Explanation of how this repo processes predictions.
It processed in 3 stages. First in run_ner.py, it outputs predictions for both eval (dev) and test sets in output_dir/{num_k}_label_eval.txt. This is of the form of the raw labels. Then, in ner_detokenize, there is a mapping process that maps the labels back to non-tokenized words to be compared with the original text. This masking process is done with 'tok_to_orig', which is a mask of the original text in word-piece. For example, a 3 word sentence would be masked as  [-1, 0, 0, 0, 0, 0, 1, 1, 1, 2, -1], where -1 denotes special tokens like CLS and each word is indexed by its number. ner_detokenize also takes the label of the first sub-token in a token as the label of that token. 
Finally, ner_dentokenize writes out this final label in NER_result_conll_test.txt is basically the output of the predictions.
