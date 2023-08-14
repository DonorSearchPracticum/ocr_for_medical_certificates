## Оптическое распознавание информации в медицинских справках

Данное OCR-решение разработано для распознавания данных с фото медицинских справок для последующей интеграции в систему [проекта по поиску доноров крови](https://donorsearch.org/).

Было необходимо успешно получить следующую информацию:
* Дата донации
* Факт оплаты донации
* Класс крови донации

---

### Итог работы

Готовый микросервис для распознавания нужной информации, отлично работающий с большинством фотографий, кроме достаточно "размытых". Работает на основе библиотеки [docTR](https://mindee.github.io/doctr/index.html) и с использованием постобработки с помощью регулярных выражений.

Качество решения в рамках метрики *accuracy* по отдельным ячейкам составляет **0.91**.

Описание работы и тестирование, в том числе с интерактивным графиком, можно увидеть в [Jupiter-тетрадке](https://nbviewer.org/github/Darveivoldavara/ocr_for_medical_certificates/blob/6714f0dfc44e341a01c33353a9cf2db2719aa032/Doctr/ocr_for_medical_certificates.ipynb) на nbviewer.

---

### Загрузка сервиса (Docker)

Найти docker-контейнер с готовым микросервисом можно [здесь](https://hub.docker.com/r/rookblack/ocr_donor).

Скачать: `docker pull rookblack/ocr_donor:latest`

Запустить: `docker run -p 8000:8000 rookblack/ocr_donor`

[Файлы для самостоятельной сборки контейнера](https://github.com/Darveivoldavara/ocr_for_medical_certificates/tree/main/Docker).

---

### Обновления качества

| Дата | Значение accuracy | Изменение | 
| :---------------------- | :---------------------- | :---------------------- |
| **04.07.2023** | **0.86** | **изначальный показатель** |
| **08.07.2023** | **0.872** | **заполнение пропусков наиболее частыми соответствующими значениями** |
| **10.07.2023** | **0.879** | **подбор наилучших моделей детекции и распознавания текста** |
| **13.07.2023** | **0.904** | **адаптация скрипта к результатам работы новых моделей** |
| **16.07.2023** | **0.91** | **финальный дебаг скрипта** |

---

### Не сработало

Другие изначально рассмотренные библиотеки для распознавания текста и таблиц, такие как **Tesseract**, **EasyOCR** и **Paddle OCR** давали ощутимо более низкое качество, чем использование для этих целей предобученных нейронных сетей, представленных в **docTR**.

Также были протестированы различные методы по предобработке входящих изображений для улучшения их чёткости, и как следствие, повышения качества распознавания символов. Но все рассмотренные способы в итоге давали максимум визуальное улучшение и повышение чёткости, резкости фото, но многие символы в целом видоизменялись на некорректные, и итоговое качество их распознавания только падало по сравнию с решением без какой-либо предобработки вообще. Протестированы были следующие методы:

* Библиотека `cv2` (методы для [adaptiveThreshold()](https://docs.opencv.org/3.4.0/d7/d1b/group__imgproc__misc.html#ga72b913f352e4a1b1b397736707afcde3) с различными настройками: **ADAPTIVE_THRESH_MEAN_C** и **ADAPTIVE_THRESH_GAUSSIAN_C**) для усиления границ элементов изображения
* Библиотека `PIL` (фильтр **SHARPEN** для [ImageFilter](https://pillow.readthedocs.io/en/stable/reference/ImageFilter.html)) для повышения резкости фото
* Решения, использующие нейронные сети для апскейла и/или улучшения изображения:
  * API [Waifu2x](https://deepai.org/machine-learning-model/waifu2x)
  * Проект [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN) (дефолтная модель **RealESRGAN_x4plus** с различными настройками)

---

### Не успели

* Поробовать интегрировать в используемый метод библиотеки docTR какие-то другие модели для детекции и распознавания помимо уже встроенных (например, с [HuggingFace](https://huggingface.co/))
* Отсмотреть возможные улучшения качества работы скрипта на абсолютно всех тестовых примерах (включая обработанные с сильно высоким показателем *accuracy*)
* Протестировать иные методы для предобработки изображений

---

### Статус

Доработка: реализация асинхронной работы сервиса (**streaming**) с помощью [Celery](https://github.com/celery/celery).
