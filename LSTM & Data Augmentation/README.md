# Sentiment analysis on [Stanford Sentiment Analysis Dataset](http://nlp.stanford.edu/~socherr/stanfordSentimentTreebank.zip) using LSTM
This dataset contains just over 10,000 pieces of Stanford data from HTML files of Rotten Tomatoes. 

For example:
- The Interview was neither that funny nor that witty.
- Even if there are words like funny and witty, the overall structure is a negative type.

## Quick brief about the data files - 

1. dictionary.txt contains all phrases and their IDs, separated by a vertical line |
2. sentiment_labels.txt contains all phrase ids and the corresponding sentiment labels, separated by a vertical line.Note that you can recover the 5 classes by mapping the positivity probability using the following cut-offs: [0, 0.2], (0.2, 0.4], (0.4, 0.6], (0.6, 0.8], (0.8, 1.0] for very negative, negative, neutral, positive, very positive, respectively. Please note that phrase ids and sentence ids are not the same.
3. datasetSentences.txt contains the sentence index, followed by the sentence string separated by a tab. 

## Data Augmentation - 

We have used the following data augmentation techniques:
- 1. Random Deletion - As the name suggests, random deletion deletes words from a sentence. Given a probability parameter p, it will go through the sentence and decide whether to delete a word or not based on that random probability. 
- 2. Random Swap - The random swap augmentation takes a sentence and then swaps words within it n times, with each iteration working on the previously swapped sentence. 
- 3. Back Translation - Another popular approach for augmenting text datasets is back translation. This involves translating a sentence from our target language into one or more other languages and then translating all of them back to the original language. 

Using data augmentation we have tried to balance all the 5 classes, due to long consversion time and limit on translations constraints they are use for 10% of augemntation samples rest are equally done using random swap and back translation.

## Building the model - 

Once data augmentation is done we have train_aug and test files which has train and test data in csv format. Using this data we built LSTM based model.

LSTM model has following layers -
- (embedding): Embedding(18141, 300)
- (encoder): LSTM(300, 100, num_layers=2, batch_first=True, dropout=0.2, bidirectional=True)
- (fc): Linear(in_features=100, out_features=5, bias=True)

And hyperparameters as : embedding_dim = 300, num_hidden_nodes = 100, num_output_nodes = 5, num_layers = 2, dropout = 0.2.

We could achieve training accuracy of 100 % but validation accuracy was not performing well (May be due to simple model & complex data). So, we decided to use 5 epochs where the training accuracy is around 78 % and validation accuracy is around 35 %.

## Testing our model on 10 custom inputs

| Sentence | Prediction |
| --- | --- |
| The notion that bombing buildings is the funniest thing in the world goes entirely unexamined in this startlingly unfunny comedy . | negative |
| Visually rather stunning , but ultimately a handsome-looking bore , the true creativity would have been to hide Treasure Planet entirely and completely reimagine it . | very negative |
| Miller is playing so free with emotions , and the fact that children are hostages to fortune , that he makes the audience hostage to his swaggering affectation of seriousness . | positive |
| Schaeffer has to find some hook on which to hang his persistently useless movies , and it might as well be the resuscitation of the middle-aged character .| neutral |
| When the film ended , I felt tired and drained and wanted to lie on my own deathbed for a while . | negative |
| Full of witless jokes , dealing in broad stereotypes and outrageously unbelievable scenarios , and saddled with a general air of misogyny | negative |
| The film 's hackneyed message is not helped by the thin characterizations , nonexistent plot and pretentious visual style . | negative |
| Detox is ultimately a pointless endeavor | very negative |
| Too much of the humor falls flat . | very negative |
| "No way I can believe this load of junk . |  negative |

Although our model didn't do well on validation data, It gave decent results on our custom inputs. 
