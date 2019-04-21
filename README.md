# Тезисы докеростроения

## Не используйте в проектах для Заказчика имена сторонних образов
 
Использование имен docker-образов с префиксом, отличным от имени локального регистратора (`dh.ics.perm.ru`) может привести к существенным проблемам при разворачивании решения на площадке Заказчика:
- отсутствие интернета не даст возможность загрузить образ;
- при наличии интернета и отсутствии тега  образа установленный образ (latest) может отличаться от того, который использовали Вы при разработке системы.

В связи с этим рекомедуется в этом случае для всем образам, используемых в проектах Заказчиков
присваивать имена в домене локального регистратора (`dh.ics.perm.ru`) по шаблону:
```
dh.ics.perm.ru/<ИМЯ_ПРОЕКТА>/<ИМЯ_ОБРАЗА_В_ПРОЕКТЕ>
```
Для создания образа создайте Dockerfile и постройте на его основе собственный образ.

В простейшем случае, когда Вы используете сторонний образ (в том числе с другим именем в домене `dh.ics.perm.ru`) без изменений создается минимальный Dockerfile
```
FROM <имя_родительского_образа>:<тег>
MAINTAINER <E-mail>
```

После выполнения команды 
```
$ docker build -r dh.ics.perm.ru/<ИМЯ_ПРОЕКТА>/<ИМЯ_ОБРАЗА_В_ПРОЕКТЕ>:<ТЕГ> .
```
docker создаст указанный образ.

Три строчки в Ваш собственный образ готов!

Данный образ будет являться алисом образа  `<имя_родительского_образа>:<тег>`
Аналогичный результат можно было бы получить выполнением команды:
```
$ docker tag <имя_родительского_образа>:<тег>  dh.ics.perm.ru/<ИМЯ_ПРОЕКТА>/<ИМЯ_ОБРАЗА_В_ПРОЕКТЕ>:<ТЕГ>
```
Достоинcтва данного подхода:
- Возможность размещения алиаса образа в локальном регистраторе `dh.ics.perm.ru` и в регистраторе Заказчика.
- Поддержка работы и обновления образов на площадке Заказчика даже при отсутсвии доступа в Интенет.
- Фиксация (привязка) локального образа с конкретныму образу и тегу.
- Возможность расширения образа в дальныйшем (добаление конфигурационных файлов, вспомогтельных скриптов, ...) 


## GIT'уйте дерева сбоки образа

После создания Dockerfile и дополнительный файлов создайте GIT-репозиторий.
```
$ git init
$ git add Dockerfile ...
$ git commit -a
```
Это даст Вам следующие преимущества:
- ведение нескольких веток (версий) образа и его системы сборки, что упростит этап тестирования и поддержки различных весий образа;
- возможность привязки тегов образа к тегам системы сборки;
- возможность размещения образа на локальных (`dis-tfs`), приватных (`git-репозитории Заказчика`) и внешних (`githum.com`) репозиториях для совместного распределенного ведения образов. 

## Детализация образа

- Настройка и фиксация конфигурационных файлов.
- Добавление статических и бинарных файлов.
- Формирование начальных значений динамических файлов (баз данных, структуры файловых систем).


## Разделяй статические и динамические данные

## Кроме docker-тега latest есть еще и другие теги

## Не бойся больших объемов образов: 2Gb x 2 != 4Gb

## Не раздувай образ - используй Multistage build 

## Development Dockerfile и Production Dockerfle - две большие разницы 
