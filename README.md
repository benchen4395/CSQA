## Model Name:
DeBERTa (ensemble model)

## Affiliation:
Alibaba Group ICBU Tech

## Model Description: 
We input the pooler_output ouput embeddings in last layer to make the prediction, even if its implict but useful.

Also, we use the basic [nlpaug](https://github.com/makcedward/nlpaug) to augment the training data with synonym replacement, random insertion, random swap and random deletion by Statistics and Machine Learning, aiming to make improve model robustness for word order and stacking.

The pre-training model we used is deberta-v2-xxlarge, and the input method is [CLS] qustion [SEP] answer + 'with concept is '+ question-concept [SEP]

The loss we used is modified heated-up softmax loss derived from https://github.com/ColumbiaDVMM/Heated_Up_Softmax_Embedding.

After Finetuning with the lr=4e-6, we countinue finetuning the model with the lr=1e-6.

Although we find the addition of extra conception of the question sentence can improve the performance obviously, here we only use the provided information for seeking a better finetuning method and sentence format for commonsense question. 

Beside, we try to use the [Flan-T5](https://arxiv.org/pdf/2210.11416.pdf) to explain the conception of question-concept, for those questions whose predicted score of most reliable answer is lower than 0.7. In this way, we replace the question-concept by the explaination. This method tackle about 100 questions, and all results are choosed only by deberta-xxlarge-v2 with no other models.

So the result is evaluated on the provided question-choices pairs without extra data.

## Hyper-parameters Setting: 

In our experiments, we used the pre-trained DeBERTa-xxlarge-v2 model from https://github.com/microsoft/DeBERTa, and implement a DebertaForMultipleChoice function to the task.

The parameters are listed below:
- `EDA parameter` alpha_sr=alpha_ri=alpha_rs=alpha_rd=0.1, num_aug=4
- `classifier function` use a fc-layer
- MAX_SEQ_LENGTH = 90
- TRAIN_BATCH_SIZE = 32
- GRADIENT_ACCUMULATION_STEPS = 1
- LEARNING_RATE = 4e-6 (2 epoch)/ 1e-6 (1 epoch)
- WEIGHT_DECAY = 0.0
- ADAM_EPSILON = 1e-8
- NUM_TRAIN_EPOCHS=8
- LOGGING_STEPS = 50
