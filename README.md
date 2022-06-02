# Encoder Decoder Sequence to Sequence model Dialogue System
seq2seq model is used to build a generative model for dialogue system
<hr>

**Dataset**
The Cornell Movie-Dialogs Corpus: 
The Cornell Movie-Dialogs Corpus contains 220,579 conversational exchanges between 10,292 pairs of movie characters, 9,035 characters from 617 movies, and 304,713 total utterances. This dataset is large with a wide variety of language formality, time periods, and other variables. Our hope is that this variety will make our model responsive to a wide range of queries.

<hr>
**Encoder**: For Sequence-to-Sequence model, encoding of the source sentence is done by an Encoder RNN. It provides the initial hidden state for the decoder RNN.

For creating an encoder model, we are using Embedding layer which is created by using the embeddings created by using GloVe 50d data which converts each word into a fixed-length vector of the Embed size provided- Embed_dim: 50.

Keras dropout layer on embedding output to prevent overfitting on the training data by dropping the fraction of the input units. Here, we have used the dropping rate as 0.2

We'll utilize a bidirectional rendition of the GRU, which successfully implies there are two separate RNNs: one took care of the info grouping in normal consecutive request and the other took care of the information arrangement in switch requests. At each time point, the results of each organization are added together. Next step is to use 2 Bidirectional GRU layers with size = 50 and return_sequences = True. The output of the second GRU layer consists of the hidden states as well which can be used as initial states for the decoder.

A dropout layer is added between the two Bidirectional GRU layers to avoid overfitting after all the output of the output sequence.
<hr>

**Decoder**: The other part of the sequence-to-sequence model is the decoder. Decoder is an RNN language model that generates target sentence conditioned on encoding, So, encoder last hidden states are used to initialize the decoder.

For the decoder model, we are using the attention mechanism to focus on the specific parts of the input sequence. 
The attention layer is created using the BahdanauAttention which is based on Bahdanau et al.

For the decoder, the attention layer is used to generate the context output and attention weights. First, the Input is fed to an embedding layer to generate the fixed-length embeddings. Embedded data is then concatenated with the attention context. Resulted output is fed to GRU layer with return_sequences = True. Here, we are not using Bidirectional GRU as we are generating the output language model. The dropout layer is applied to the output of the first GRU layer to avoid overfitting and finally, we are using another GRU layer with return_sequences = False to get the last output of the output sequence. A fully connected Dense layer is used to generate the output.

<hr>
The model is trained for 150 epochs and the best epoch with minimum loss is achieved at epoch 146.
Training Loss value for Model with epochs 5, 50 and 140 has significantly decreased with the number of epochs as shown below:


<img width="636" alt="image" src="https://user-images.githubusercontent.com/35501313/171734531-4e8c3920-0392-4d74-8dc6-0486f23e2237.png">
<hr>

Response of a question after Epoch 140.

<img width="568" alt="image" src="https://user-images.githubusercontent.com/35501313/171734614-4f385570-6e11-4cf9-8767-8a4dbb28702a.png">

**Conslusion**
We can observe that the higher attention weights have been applied to the initial part of the sentence and as the number of epochs increases the attention weight distribution to the important words get better. Hence, the model is able to generate better responses.



 
