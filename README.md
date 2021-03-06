# Juppy

### Универсальный образ для работы в jupyter

Установка всех библиотек и сервисов для ML для эксперементирования с различными штуками очень сильно загаживает систему, так что я просто завернул все это в докер.

Образ доступен на докер хабе, можно скачать его и прописать алиас для запуска, но для серьезного использования рекомендую клонировать репозиторий и собрать образ самому.
> **docker pull voidwalker/juppy**
>
> alias juppy='docker stop juppy; docker rm juppy; docker run --name=juppy -it -p 8888:8888 -v `/DIRECTORY/FOR/NOTEBOOKS`:/opt/notebooks -v juppy /bin/bash -c "/opt/conda/bin/jupyter notebook --ip=0.0.0.0 --port=8888"'

## Сборка

**Самостоятельная сборка образа позволит устанавливать дополнительные пакеты в окружение в процессе использования образа, и `не терять их при пересоздании контейнера`**

Устанавливать пакеты можно с помощью pip, запустив терминал в jupyter в браузере (`new -> terminal -> "pip install ..."`).

- Клонируйте этот репозиторий
- Запустите скрипт сборки `./build.sh`
- Подождите пока образ соберется - это может занять некоторое время
- Когда образ соберется, скрипт запросит 3 параметра ( последние два проще всего оставить по умолчанию):
  - `Directory for notebooks` -  путь к директории на хосте, где будут храниться jupyter-ноутбуки
  - `Directory for cache - python packages, etc.` - директория с "кэшем" - сюда будут устанавливаться все пакеты, это позволяет не терять нужные для работы дополнительные пакеты, установленные пользователем
  - `Port for jupyter` - порт, на котором будет запущен jupyter
- После окончания работы, скрипт выведет "------" и строку, содержащую алиас, который можно скопировать в свой .bashrc/.zshrc/etc.

В папке, указанной для хранения кэша (по умолчанию - `~/.juppy`) будет лежать файл `jupyter_notebook_config.py` - это файл с конфигом jupyter, в нем можно указать пароль для входа или вообще отключить пароли.

> После этого у вас появится команда `juppy`, запускающая контейнер с jupyter, доступным в бразуере по адресу `127.0.0.1:8888`

Если вы не указывали пароль в файле с конфигом, то для входа так же понадобится передать в url токен, который будет выведен в терминал при запуске контейнера (можно просто скопировать распечатанный  url и заменить в нем хост на localhost)

Если в папке, которую вы указали для хранения ноутбуков, есть файл requirements.txt, то зависимости из него установятся автоматически.
