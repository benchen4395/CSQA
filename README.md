## Model Name:
DeBERTa (single model)

## Affiliation:
Alibaba Group ICBU Tech

## Model Description: 
We input the mean values of ouput embeddings in last layer to make the prediction, as we believe each token embedding are helpful to common-sense reasoning.

Also, we use the basic [EDA](https://arxiv.org/abs/1901.11196) to augment the training data with synonym replacement, random insertion, random swap and random deletion, aiming to make improve model robustness for word order and stacking

The pre-training model we used is deberta-v2-xxlarge, and the input method is [CLS] qustion [SEP] 'concept is '+ question-concept +'so the answer is ' + answer [SEP]

The result is evaluated on the provided question-choices pairs without extra data.

## Hyper-parameters Setting: 

In our experiments, we used the pre-trained ALBERT-xxlarge-v2 model from https://huggingface.co/microsoft/deberta-v2-xxlarge, and implement a DebertaForMultipleChoice function to the task.

The parameters are listed below:
- `EDA parameter` alpha_sr=alpha_ri=alpha_rs=alpha_rd=0.1, num_aug=4
- `classifier function` use a fc-layer
- MAX_SEQ_LENGTH = 90
- TRAIN_BATCH_SIZE = 8
- GRADIENT_ACCUMULATION_STEPS = 1
- LEARNING_RATE = 4e-6
- WEIGHT_DECAY = 0.0
- ADAM_EPSILON = 1e-8
- NUM_TRAIN_EPOCHS=2
- LOGGING_STEPS = 50
