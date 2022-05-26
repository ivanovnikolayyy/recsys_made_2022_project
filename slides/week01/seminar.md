# Семинар 1. Первый эксперимент

На этом семинаре мы познакомимся с сервисом рекомендаций _botify_ и научимся запускать и анализировать A/B эксперименты.
Первая версия рекомендера на каждый запрос отдает случайный трек.
Такие рекомендации скорее всего не очень релевантны.
Попробуем в качестве рекомендаций возвращать случайный трек того же исполнителя, что и предыдущий.

[!] Чтобы найти пропуски в коде, которые нужно заполнить, можно поискать в проекте строки "TODO Seminar 1" 

### План действий

1. Создаем новую базу Redis, в которой будут данные о треках исполнителей
    1. Конфигурируем новый коннект к редису в конфиге сервиса `config.json`. 
       Прописываем в нем новую базу DB=1.
    2. Создаем коннект к новой базе в `server.py` по аналогии с `tracks_redis`.
    
2. Загружаем в новую базу данные о треках исполнителей 
    1. Используя данные о треках в `botify.track.Catalog`, находим все треки каждого исполнителя.
       Загружаем данные в redis с ключом "имя исполнителя" и значением "список треков исполнителя"
       Это можно сделать по аналогии с методом `upload_tracks`.
        
3.  Реализуем новый рекомендер `botify.recommenders.sticky_artist.StickyArtist`, работающий по принципу:
    1. Спрашиваем в redis информацию по предыдущему треку, достаем имя исполнителя.
    2. Спрашиваем в redis все треки этого исполнителя.
    3. Рекомендуем случайный из найденных треков.
    
4. Запускаем A/B эксперимент.
    1. В `botify.experiment.Experiments` создаем эксперимент `STICKY_ARTIST`.
    2. В `botify.server.NextTrack` используем этот эксперимент для выбора рекомендера между `Random` и `StickyArtist`.
    3. Запускаем рекомендер и симулятор.
    4. Скачиваем логи сервиса.
   
4. Используем ноутбук `Week1Seminar.ipynb` для анализа.
    1. Запускаем анализатор для эксперимента `AA`.
    2. Запускаем анализатор для эксперимента `STICKY_ARTIST`.

### Вопросы

1. Какие проблемы есть у рекомендера `StickyArtist`? 
   Какие есть простые фиксы?
   
2. Ожидаемые ли результаты у эксперимента `STICKY_ARTIST`?