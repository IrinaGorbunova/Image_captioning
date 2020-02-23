# Image Captioning

В данном репозитории представлены результаты моей работы над дипломным проектом Image Captioning в рамках курса Deep Learning School 2. Проект включает в себя 7 ноутбуков, содержание и результаты которых кратко проиллюстрированы в следующей таблице:

| Version | Dataset | Encoder | Embeddings | Attention | Best BLEU | Beam search | Comment |
|:-------:|:-------:|:-------:|:----------:|:---------:|:---------:|:-----------:|:-------:|
| v1      | MS COCO | Inception 3 | glove 300d 6B | - | 25.02 | 26.6 (k=3) |
| v2      | MS COCO | Inception 3 | crawl 300d 2M | - | 25.08 | 27.1 (k=3) | + fc |
| v3      | Flickr 30 | Inception 3 | crawl 300d 2M | I | 18.29 | | долго |
| v4      | Flickr 30 | Resnext 101 | crawl 300d 2M | I | 20.52 | | тяжелая |
| v5      | Flickr 30 | Resnet 101 | crawl 300d 2M | I | 20.87 | 
| v6      | Flickr 30 | Resnet 101 | crawl 300d 2M | - | 18.49 | 
| v7      | Flickr 30 | Resnet 101 | crawl 300d 2M | II | 20.93 | | bmm |

Глобально можно выделить 2 блока:

1. Работа с датасетом MS COCO - базовый вариант, построение RNN декодера для генерации заголовков на основании готовых эмбеддингов картинок.
2. Работа с датасетом Flickr 30 - идея заключалась в реализации механизма внимания в декодере, а для этого требовалось получить больше информации от картинок. Выбор пал на датасет Flickr 30, так как он меньше MS COCO. В данном направлении рассмотрены 3 разных CNN модели в качестве энкодера, а также 2 вида внимания.

Для оценки качества полученных моделей использовалась метрика BLEU. Хотя сравнивать между собой модели, обученные на разных датасетах, довольно сложно, ведь в первом случае BLEU рассчитывался примерно на 10000 элементах валидационного датасета, а во втором всего на 1000, да и элементы совсем другие. 
Поэтому в каждом ноутбуке приводятся примеры генерации заголовков к случайно скачанным из интернета изображениям, по которым можно получить более объективную информацию.

Также для каждого датасета выбран фиксированный пример из валидационного набора, за предсказаниями для которого можно следить во время обучения моделей. По этому критерию модели, обученные на Flickr 30 намного ближе к оригиналу. Хотя BLEU существенно выше у MS COCO.

### Небольшие наблюдения:

1. В этом случае получилось примерно одинаково:

| Version | Dataset | Encoder | Embeddings | Attention | Best BLEU | Beam search |
|:-------:|:-------:|:-------:|:----------:|:---------:|:---------:|:-----------:|
| v1      | MS COCO | Inception 3 | glove 300d 6B | - | 25.02 | 26.6 (k=3) |
| v2      | MS COCO | Inception 3 | crawl 300d 2M | - | 25.08 | 27.1 (k=3) | 

2. Свое предпочтение я отдаю Resnet 101:

| Version | Dataset | Encoder | Embeddings | Attention | Best BLEU | Comment |
|:-------:|:-------:|:-------:|:----------:|:---------:|:---------:|:-------:|
| v3      | Flickr 30 | Inception 3 | crawl 300d 2M | I | 18.29 | долго |
| v4      | Flickr 30 | Resnext 101 | crawl 300d 2M | I | 20.52 | тяжелая |
| **v5**      | **Flickr 30** | **Resnet 101** | **crawl 300d 2M** | **I** | **20.87** |

3. С вниманием всегда лучше!

| Version | Dataset | Encoder | Embeddings | Attention | Best BLEU |
|:-------:|:-------:|:-------:|:----------:|:---------:|:---------:|
| **v5**      | **Flickr 30** | **Resnet 101** | **crawl 300d 2M** | **I** | **20.87** | 
| v6      | Flickr 30 | Resnet 101 | crawl 300d 2M | - | 18.49 | 
| **v7**      | **Flickr 30** | **Resnet 101** | **crawl 300d 2M** | **II** | **20.93** |


Сохраненные веса моделей можно скачать по ссылке:
https://drive.google.com/drive/folders/18P3pPTviBJ0T4h7DtkBYSVOcoxASmYxS?usp=sharing

### Ну и как же без примера напоследок:

![Иллюстрация к проекту](https://github.com/IrinaGorbunova/Image_captioning/blob/master/img.jpg)

А вот варианты заголовков от разных моделей:

**v1** : "a man riding a skateboard down a ramp ."

**v2** : "a man riding a skateboard up the side of a ramp ."

**v3** : "a man jumps off a diving board into a pool ."

**v4** : "a man in a green shirt and black shorts is doing a trick on a ramp ."

**v5** : "a man wearing a green shirt and black pants is doing a trick on a skateboard ."

**v6** : "a man in a black shirt is doing a trick on a skateboard ."

**v7** : "a young man in a green shirt is skateboarding on a skateboard ."

Модели, обученные на Flickr 30, почти всегда начинают описывать, кто во что одет, такие забавные.
