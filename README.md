# Was Shakespeare a Woman?
(версия на русском ниже)

## In a nutshell
Inspired by Elizabeth Winkler’s 2019 article for the Atlantic, this project adds to the centuries-old debate about William Shakespeare’s identity. We trained a variety of models (see Models) on three different corpora. FIrst, to see that models are able to pick up on stylistic differences between authors, we trained models on stories by Arthur Conan Doyle and Agatha Christie, which was successful (see Results). Second, we collected a corpus of modern-day romances from male and female authors, and trained models to distinguish between them, which was less successful. Finally, using a corpus of letters in Old English, we trained models to differentiate between those written by women and those written by men, which was comparatively successful. Using the models trained on the letters corpus, we tried to predict Shakespeare's gender, to inconclusive results. 

## Datasets
1. Doyle & Christie dataset: train: sentences from Doyle and Christies mystery stories, 18k lines; test: sentences from their non-mystery stories, 2k lines; targets are Doyle or Christie.
3. English letters dataset: train: sentences from letters in Old English [corpus](https://ota.bodleian.ox.ac.uk/repository/xmlui/handle/20.500.12024/2461), 5k lines; test: sentences from the same corpus, 500 lines; targets are male or female. 
4. Modern dataset: train: sentences from modern romance novels, 50k lines; test: 5k lines; targets are male or female.

### Preprocessing (TBD)

## Models
1. Classic ML: logistic regression + tf-idf, logistic regression + counts of BOW, SVM + tf-idf
2. CNN: word2vec-google-news-300e embeddings, filters 2, 3, 4
3. A siamese neural network: the model receives a pair of sentences that are written either by two women, two men or a man and a woman. The targets in this case are 0 for 'written by people of different genders' or 1 for 'written by people of the same gender'. We used pre-trained *word2vec-google-news-300* embeddings. This was a "just for fun" experiment that ended up working very badly. 
4. BERT: we just fine-tuned *bert-base-cased* model

## Results

Accuracy scores:
| model                 | Doyle & Christie dataset | English letters dataset | Modern dataset|
|-----------------------|--------------------------|-------------------------|---------------|
|bow + logreg           |0.74|0.87|0.52|
|tfidf + logreg         |0.74|0.86|0.52|
|svm                    |0.75|0.88|0.52|
|siamese                |0.50|0.53|0.50|
|CNN                    |0.57|0.73|0.50|
|bert                   |0.80|0.80|0.51|

Shakespeare scores:
| model                 |Logit Score|Decision|
|-----------------------|-----|--------|
|bow + logreg           |0.5| ? |
|tfidf + logreg         |0.45| female |
|svm                    |0.48|female|
|CNN                    |0.55|male|
|bert                   |0.38|female|

The scores are very uncertain, so we consider results inconclusive.

## Введение (в процессе)

## Датасеты
1. Doyle & Christie dataset: 18k строк в трейне, 2k строк в тесте, сбалансированный. В трейне детективы, в тесте не-детективы. 
2. English letters dataset: ~10k строк, не сбалансирован. В моделях везде используется сбалансированная версия. Там не совсем но пофикшен староанглийский
3. Modern dataset: много строк, не сбалансирован. В моделях везде используется сбалансированная версия.

## Модели
1. Классический ML: логистическая регрессия + tf-idf, логистическая регрессия + мешок слов, SVM + tf-idf
2. ! CNN: эмбеддинги word2vec-google-news-300, фильтры 2, 3, 4
3. Сиамская нейросеть: модель получает пару предложений, и учится определять, была ли ли они написаны людьми одного пола или разных. Мы использовали готовые эмбеддинги *word2vec-google-news-300*. У нас не было никаких надежд на эту модель, и она их оправдала: всё работало очень плохо.
4. BERT: мы файн-тюнили модель bert-base-cased

## Результаты
| model                 | Doyle & Christie dataset | English letters dataset | Modern dataset|
|-----------------------|--------------------------|-------------------------|---------------|
|bow + logreg           |0.74|0.87|0.52|
|tfidf + logreg         |0.74|0.86|0.52|
|svm                    |0.75|0.88|0.52|
|siamese                |0.50|0.53|0.50|
|CNN                    |0.57|0.73|0.50|
|bert                   |0.80|0.80|0.51|

## что произошло
Мы занимаемся author profiling'ом с целью выяснить, не был ли Шекспир женщиной. Сначала мы попробовали обучить все выбранные модели на детективах Дойля и Кристи и обучить на их же не детективах, чтобы проверить, что модели вообще могут подцепить какие-то стилистические особенности. Потом мы собрали датасет из писем на староанглийском (брать пьесы было нельзя, потому что женщины их не публиковали), чтобы проверять Шекспира на языке Шекспира. Еще мы сделали датасет с современными текстами с надеждой, что хотя бы на нём получится что-то сносное. (Не получилось.)

