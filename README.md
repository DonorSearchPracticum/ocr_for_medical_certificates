## Оптическое распознавание информации в медицинских справках

Данное OCR-решение разработано для распознавания данных с фото медицинских справок для последующей интеграции в систему [проекта по поиску доноров крови](https://donorsearch.org/)

Было необходимо успешно получить следующую информацию:
* Дата донации
* Факт оплаты донации
* Класс крови донации

---

### Итог работы

Готовый микросервис для распознавания нужной информации

Качество решения в рамках метрики *accuracy* по отдельным ячейкам составляет **0.904**

---

### Загрузка сервиса (Docker)

Найти docker-контейнер с готовым микросервисом можно [здесь](https://hub.docker.com/repository/docker/darveivoldavara/ocr_for_medical_certificates/)

Скачать: `docker pull darveivoldavara/ocr_for_medical_certificates:v1`

Запустить: `docker run -p 8501:8501 84f61c063653`

---

### Обновления качества

| Дата | Значение accuracy | Изменение | 
| :---------------------- | :---------------------- | :---------------------- |
| **04.07.2023** | **0.86** | **изначальный показатель** |
| **08.07.2023** | **0.872** | **заполнение пропусков наиболее частыми соответствующими значениями** |
| **10.07.2023** | **0.879** | **подбор наилучших моделей детекции и распознавания текста** |
| **13.07.2023** | **0.904** | **адаптация скрипта к результатам работы новых моделей** |