# Что такое API. REST и RESTful. Django REST Framework.

[/не отображается/]: #(![]&#40;https://www.meme-arsenal.com/memes/6200d14d795eab11d26a3afabed68439.jpg&#41;)

## Что же такое API?

Итак, начнём с определения. API (Application Programming Interface) — это интерфейс программирования, интерфейс создания
приложений.

В нашем конкретном случае под API практически всегда будет подразумеваться REST API, о котором мы поговорим дальше.  
Сейчас для нас - это endpoint (url, на который можно отправить запрос), который выполняет какие-либо действия или 
возвращает нам информацию.

## Что такое REST?

[/не отображается/]: # (![]&#40;https://i1.wp.com/www.ybouglouan.pl/wp-content/uploads/2017/04/rest_api_sortof.jpg&#41;)

REST (Representational State Transfer — «передача состояния представления») - это архитектурный стиль
(рекомендации к разработке), но это в теории, практику рассмотрим дальше.

![](https://automated-testing.info/uploads/default/original/2X/e/eaf77634076d45e18c501abf936a9a8ad1913bb4.png)

### Свойства REST архитектуры.

Свойства архитектуры, которые зависят от ограничений, наложенных на REST-системы:

1. **Client-Server**. Система должна быть разделена на клиентов и на сервер(ы). Разделение интерфейсов означает, что
   клиенты не связаны с хранением данных, которое остается внутри каждого сервера, так что мобильность кода
   клиента улучшается. Серверы не связаны с интерфейсом пользователя или состоянием, так что серверы могут быть проще и
   масштабируемы. Серверы и клиенты могут быть заменяемы и разрабатываться независимо, пока интерфейс не изменяется.

2. **Stateless**. Сервер не должен хранить какой-либо информации о клиентах. В запросе должна храниться вся необходимая
   информация для обработки запроса и, если необходимо, идентификации клиента.

3. **Cache**․ Каждый ответ должен быть отмечен, является ли он кэшируемым или нет, для предотвращения повторного
   использования клиентами устаревших или некорректных данных в ответ на дальнейшие запросы.

4. **Uniform Interface**. Единый интерфейс определяет интерфейс между клиентами и серверами. Это упрощает и отделяет
   архитектуру, которая позволяет каждой части развиваться самостоятельно.

   Четыре принципа единого интерфейса:

   4.1) **Identification of resources (основан на ресурсах)**. В REST ресурсом является все то, чему можно дать имя.
   Например, пользователь, изображение, предмет (майка, голодная собака, текущая погода) и т. д. Каждый ресурс в REST
   должен быть идентифицирован посредством стабильного идентификатора, который не меняется при изменении состояния
   ресурса. Идентификатором в REST является URI.

   4.2) **Manipulation of resources through representations. (Манипуляции над ресурсами через представления)**.
   Представление в REST используется для выполнения действий над ресурсами. Представление ресурса представляет собой
   текущее или желаемое состояние ресурса. Например, если ресурсом является пользователь, то представлением может
   являться XML или HTML описание этого пользователя.

   4.3) **Self-descriptive messages (само-документируемые сообщения)**. Под само-описательностью имеется в виду, что
   запрос и ответ должны хранить в себе всю необходимую информацию для их обработки. Не должны быть дополнительные
   сообщения или кэши для обработки одного запроса. Другими словами, отсутствие состояния, сохраняемого между запросами к
   ресурсам. Это очень важно для масштабирования системы.

   4.4) **HATEOAS (hypermedia as the engine of application state)**. Статус ресурса передается через содержимое body,
   параметры строки запроса, заголовки запросов и запрашиваемый URI (имя ресурса). Это называется гипермедиа (или
   гиперссылки с гипертекстом). HATEOAS также означает, что в случае необходимости ссылки могут содержаться в теле
   ответа (или заголовках) для поддержки URI, извлечения самого объекта или запрошенных объектов.

5. Layered System. В REST допускается разделить систему на иерархию слоев, но с условием, что каждый компонент может
   видеть компоненты только непосредственно следующего слоя. Например, если вы вызываете службу PayPal, а она в свою
   очередь вызывает службу Visa, вы о вызове службы Visa ничего не должны знать.

6. Code-On-Demand (опционально). В REST позволяется загрузка и выполнение кода или программы на стороне клиента.

Если выполнены первые 4 пункта и не нарушены 5 и 6, такое приложение называется **RESTful**

**Важно!** Сама архитектура REST не привязана к конкретным технологиям и протоколам, но в реалиях современного WEB,
построение RESTful API почти всегда подразумевает использование HTTP и каких-либо распространенных форматов
представления ресурсов, например, JSON, или менее популярного сегодня XML.

### Идемпотентность

![](http://risovach.ru/upload/2015/12/mem/kot-bezyshodnost_100253424_orig_.jpg)

С точки зрения RESTful-сервиса операция (или вызов сервиса) идемпотентна тогда, когда клиенты могут делать один и тот
же вызов неоднократно при одном и том же результате на сервере. Другими словами, создание большого количества идентичных
запросов имеет такой же эффект, как и один запрос. Заметьте, что в то время, как идемпотентные операции производят один
и тот же результат на сервере, ответ сам по себе может не быть тем же самым (например, состояние ресурса может
измениться между запросами).

Методы PUT и DELETE по определению идемпотентны. Тем не менее есть один нюанс с методом DELETE. Проблема в том, что
успешный DELETE-запрос возвращает статус 200 (OK) или 204 (No Content), но для последующих запросов будет все время
возвращать 404 (Not Found). Состояние на сервере после каждого вызова DELETE то же самое, но ответы разные.

Методы GET, HEAD, OPTIONS и TRACE определены как безопасные. Это означает, что они предназначены только для получения
информации и не должны изменять состояние сервера. Они не должны иметь побочных эффектов, за исключением безобидных
эффектов таких как: логирование, кеширование, показ баннерной рекламы или увеличение веб-счетчика.

По определению, безопасные операции идемпотентны, так как они приводят к одному и тому же результату на сервере.
Безопасные методы реализованы как операции только для чтения. Однако безопасность не означает, что сервер должен
возвращать тот же самый результат каждый раз.

### Коды состояний HTTP (основные)

![](http://img1.reactor.cc/pics/post/http-status-code-it-http-%D0%BA%D0%BE%D1%82%D1%8D-4397611.jpeg)

Полный список кодов состояний [тут](https://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml).

**1xx: Information**

   100: Continue

**2xx: Success**

   200: OK

   201: Created

   202: Accepted

   204: No Content

**3xx: Redirect**

   301: Moved Permanently

   307: Temporary Redirect

**4xx: Client Error**

   400: Bad Request

   401: Unauthorized

   403: Forbidden

   404: Not Found

**5xx: Server Error**

   500: Internal Server Error

   501: Not Implemented

   502: Bad Gateway

   503: Service Unavailable

   504: Gateway Timeout

### Postman

На практике обычно backend-разработчики вообще не имеют отношения к тому, что происходит на фронте (если ты не 
fullstack :) ). А только подготавливают для фронта API для различных действий, чаще всего CRUD.

Для проверки работоспособности API чаще всего используется **postman** скачать можно [ТУТ](https://www.getpostman.com/)

Это программа, которая позволяет создавать запросы любой сложности к серверу. Рекомендую тщательно разобраться, как этим
пользоваться.

### Общая информация.

Хоть REST и не является протоколом, но в современном вебе это почти всегда HTTP и JSON.

### JSON

JSON (JavaScript Object Notation) - текстовый формат обмена данными, легко читается, очень похож на словарь в Python.

## Как это работает на практике и при чём тут Django?

Для Django существует несколько различных пакетов для применения REST архитектуры, но основным является **Django REST
Framework** дока [тут](https://www.django-rest-framework.org/).

### Установка

```pip install djangorestframework```

Не забываем добавить в INSTALLED_APPS **'rest_framework'**

## Сериалайзеры

Сериалайзер в DRF - это класс для преобразования данных в требуемый формат (обычно JSON).

Допустим, у нас есть такая модель:

```python
from django.db import models
from pygments.lexers import get_all_lexers
from pygments.styles import get_all_styles

LEXERS = [item for item in get_all_lexers() if item[1]]
LANGUAGE_CHOICES = sorted([(item[1][0], item[0]) for item in LEXERS])
STYLE_CHOICES = sorted([(item, item) for item in get_all_styles()])


class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    code = models.TextField()
    linenos = models.BooleanField(default=False)
    language = models.CharField(choices=LANGUAGE_CHOICES, default='python', max_length=100)
    style = models.CharField(choices=STYLE_CHOICES, default='friendly', max_length=100)

    class Meta:
        ordering = ['created']
```

Мы можем описать сериалайзер так:

```python
from rest_framework import serializers
from snippets.models import Snippet, LANGUAGE_CHOICES, STYLE_CHOICES


class SnippetSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)
    language = serializers.ChoiceField(choices=LANGUAGE_CHOICES, default='python')
    style = serializers.ChoiceField(choices=STYLE_CHOICES, default='friendly')

    def create(self, validated_data):
        """
        Create and return a new `Snippet` instance, given the validated data.
        """
        return Snippet.objects.create(**validated_data)

    def update(self, instance, validated_data):
        """
        Update and return an existing `Snippet` instance, given the validated data.
        """
        instance.title = validated_data.get('title', instance.title)
        instance.code = validated_data.get('code', instance.code)
        instance.linenos = validated_data.get('linenos', instance.linenos)
        instance.language = validated_data.get('language', instance.language)
        instance.style = validated_data.get('style', instance.style)
        instance.save()
        return instance
```

Как мы можем этим пользоваться? В shell:

```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer

snippet = Snippet(code='foo = "bar"\n')
snippet.save()

snippet = Snippet(code='print("hello, world")\n')
snippet.save()

serializer = SnippetSerializer(snippet)
serializer.data
# {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
```

Преобразование:

```python
import json

string = json.dumps(serializer.data)  # Преобразовать JSON в строку
# {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
json.loads(string)  # # Преобразовать cтроку в JSON
# {'id': 2, 'title': '', 'code': 'print("hello, world")\n', 'linenos': False, 'language': 'python', 'style': 'friendly'}
```

## Поля и их особенности

Дока [тут](https://www.django-rest-framework.org/api-guide/fields/)

Любое из полей может иметь такие аргументы:

`read_only` - поле только для чтения. Используется для полей, которые не планируются к заполнению (например, время
создания комментария), но планируются к чтению (например, отобразить, когда был написан комментарий). Такие поля не
принимаются при создании или изменении. По дефолту False.

`write_only` - ровно наоборот. Поля, не планируемые для отображения, но необходимые для записи (пароль, номер карточки,
и т. д.). По дефолту False.

`required` - обязательность поля. Поле, которое можно не указывать при создании/изменении, но его же может не быть при
чтении, допустим, отчества. По дефолту True.

`default` - значение по умолчанию, если не указано ничего другого. Не поддерживается при частичном обновлении.

`allow_null` - позволить значению поля быть None. По дефолту False.

`source` - поле, значение которого необходимо получить в модели, допустим, при помощи какого-то метода (вычисление 
полного адреса из его частей при помощи метода модели и декоратора @property, `CharField(source='get_full_address')`), 
или из какого-то вложенного объекта (Foreign Key на юзера, но необходим только его имейл, а не целый
объект, `EmailField(source='user.email')`). Имеет спец значение: `*` обозначает, что источник данных будет передан позже,
тогда его нужно будет указать в необходимых методах. По дефолту - это имя поля.

`validators` - список валидаторов, о нём поговорим ниже.

`error_messages` - словарь с кодом ошибок.

Есть и другие, но эти наиболее используемые.

У разных полей могут быть свои атрибуты, такие как максимальная длина, или количество знаков после запятой.

Виды полей по аналогии с моделями и формами могут быть практически какими угодно, за деталями в доку.

### Специфичные поля

`ListField` - поле для передачи списка.
Сигнатура `ListField(child=<A_FIELD_INSTANCE>, allow_empty=True, min_length=None, max_length=None)`

```python
scores = serializers.ListField(
    child=serializers.IntegerField(min_value=0, max_value=100)
)
```

DictField - поле для передачи словаря. Сигнатура `DictField(child=<A_FIELD_INSTANCE>, allow_empty=True)`

```python
document = DictField(child=CharField())
```

HiddenField - скрытое поле, может быть нужно для валидаций.

```python
modified = serializers.HiddenField(default=timezone.now)
```

`SerializerMethodField` -  поле, основанное на методе.

Сигнатура: `SerializerMethodField(method_name=None)`, `method_name` - название метода, по дефолту `get_<field_name>`

```python
from django.contrib.auth.models import User
from django.utils.timezone import now
from rest_framework import serializers


class UserSerializer(serializers.ModelSerializer):
    days_since_joined = serializers.SerializerMethodField()

    class Meta:
        model = User

    def get_days_since_joined(self, obj):
        return (now() - obj.date_joined).days
```

## Валидация

```python
serializer.is_valid()
serializer.validated_data
# OrderedDict([('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])
serializer.save()
# <Snippet: Snippet object>
# True
```

По аналогии с формами мы можем добавить валидацию каждого отдельного поля при помощи метода `validate_<field_name>`

```python
from rest_framework import serializers


class BlogPostSerializer(serializers.Serializer):
    title = serializers.CharField(max_length=100)
    content = serializers.CharField()

    def validate_title(self, value):
        """
        Check that the blog post is about Django.
        """
        if 'django' not in value.lower():
            raise serializers.ValidationError("Blog post is not about Django")
        return value
```

Возвращает значение или возбуждает ошибку валидации.

Также валидация может быть осуществлена на уровне объекта. Метод `validate()`.

```python
from rest_framework import serializers


class EventSerializer(serializers.Serializer):
    description = serializers.CharField(max_length=100)
    start = serializers.DateTimeField()
    finish = serializers.DateTimeField()

    def validate(self, data):
        """
        Check that start is before finish.
        """
        if data['start'] > data['finish']:
            raise serializers.ValidationError("finish must occur after start")
        return data
```

Также можно прописать валидаторы как отдельные функции:

```python
def multiple_of_ten(value):
    if value % 10 != 0:
        raise serializers.ValidationError('Not a multiple of ten')


class GameRecord(serializers.Serializer):
    score = IntegerField(validators=[multiple_of_ten])
    ...
```

Или указать в Meta, используя уже существующие валидаторы:

```python
class EventSerializer(serializers.Serializer):
    name = serializers.CharField()
    room_number = serializers.IntegerField(choices=[101, 102, 103, 201])
    date = serializers.DateField()

    class Meta:
        # Each room only has one event per day.
        validators = [
            UniqueTogetherValidator(
                queryset=Event.objects.all(),
                fields=['room_number', 'date']
            )
        ]
```

Также мы можем передать в сериалайзер список или queryset из объектов, указав при этом атрибут `many=True`

```python
serializer = SnippetSerializer(Snippet.objects.all(), many=True)
serializer.data
# [OrderedDict([('id', 1), ('title', ''), ('code', 'foo = "bar"\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 2), ('title', ''), ('code', 'print("hello, world")\n'), ('linenos', False), ('language', 'python'), ('style', 'friendly')]), OrderedDict([('id', 3), ('title', ''), ('code', 'print("hello, world")'), ('linenos', False), ('language', 'python'), ('style', 'friendly')])]
```

По аналогии с Формами и ModelForm, у сериалайзеров существуют ModelSerializer.

```python
class SnippetSerializer(serializers.ModelSerializer):
    class Meta:
        model = Snippet
        fields = ['id', 'title', 'code', 'linenos', 'language', 'style']
```

Если создать сериалайзер в таком виде, то:

```python
from snippets.serializers import SnippetSerializer

serializer = SnippetSerializer()
print(repr(serializer))
# SnippetSerializer():
#    id = IntegerField(label='ID', read_only=True)
#    title = CharField(allow_blank=True, max_length=100, required=False)
#    code = CharField(style={'base_template': 'textarea.html'})
#    linenos = BooleanField(required=False)
#    language = ChoiceField(choices=[('Clipper', 'FoxPro'), ('Cucumber', 'Gherkin'), ('RobotFramework', 'RobotFramework'), ('abap', 'ABAP'), ('ada', 'Ada')...
#    style = ChoiceField(choices=[('autumn', 'autumn'), ('borland', 'borland'), ('bw', 'bw'), ('colorful', 'colorful')...
```

Чаще всего вы будете пользоваться именно ModelSerializer.

### Вложенные сериалайзеры:

Сериалайзер может быть полем другого сериалайзера. Такие сериалайзеры называются вложенными.

```python
class UserSerializer(serializers.Serializer):
    email = serializers.EmailField()
    username = serializers.CharField(max_length=100)


class CommentSerializer(serializers.Serializer):
    user = UserSerializer()
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```

Совмещаем с предыдущими знаниями и получаем вложенное поле с атрибутом `many=True`, а значит оно принимает список или
queryset таких объектов:

```python
class CommentSerializer(serializers.Serializer):
    user = UserSerializer(required=False)
    edits = EditItemSerializer(many=True)  # A nested list of 'edit' items.
    content = serializers.CharField(max_length=200)
    created = serializers.DateTimeField()
```

### Передача данных в разные стороны:

Обратите внимание, что когда мы обрабатываем данные, полученные от пользователя (например запрос), то мы передаём 
данные в сериалайзер через атрибут, `data=` и после этого обязаны провалидировать данные, так как там могут быть ошибки:

```python
serializer = CommentSerializer(data={'user': {'email': 'foobar', 'username': 'doe'}, 'content': 'baz'})
serializer.is_valid()
# False
serializer.errors
# {'user': {'email': ['Enter a valid e-mail address.']}, 'created': ['This field is required.']}
```

Если ошибок нет, то данные будут находиться в атрибуте `validated_data`.

Но если мы сериализуем данные, которые мы взяли из базы, то у нас нет необходимости их валидировать. Мы передаём их без
каких-либо атрибутов, данные будут находиться в атрибуте `data`:

```python
comment = Comment.objects.first()
serializer = CommentSerializer(comment)
serializer.data
```

### Метод save()

У ModelSerializer по аналогии с ModelForm есть метод `save()`, но в отличие от ModelForm дополнительные данные
можно передать прямо в атрибуты метода `save()`.

```python
e = EventSerializer(data={'start': "05/05/2021", 'finish': "06/05/2021"})
e.save(description='bla-bla')
```

## Связи в сериалайзерах

Все мы знаем, что бывают связи в базе данных. Данные нужно каким-то образом получать, но в случае сериализации нам
часто нет необходимости получать весь объект, а нужны, допустим, только `id` или название. DRF это предусмотрел.

Предположим, у нас есть вот такие модели:

```python
class Album(models.Model):
    album_name = models.CharField(max_length=100)
    artist = models.CharField(max_length=100)


class Track(models.Model):
    album = models.ForeignKey(Album, related_name='tracks', on_delete=models.CASCADE)
    order = models.IntegerField()
    title = models.CharField(max_length=100)
    duration = models.IntegerField()

    class Meta:
        unique_together = ['album', 'order']
        ordering = ['order']

    def __str__(self):
        return '%d: %s' % (self.order, self.title)
```

Чтобы получить в сериалайзере альбома все его треки, мы можем сделать, например, так:

```python
class TrackSerializer(serializers.ModelSerializer):
    class Meta:
        model = Track
        fields = ['title', 'duration']


class AlbumSerializer(serializers.ModelSerializer):
    tracks = TrackSerializer(many=True)

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

Но есть и другие варианты получения данных.

### StringRelatedField()

```python
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.StringRelatedField(many=True)

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

Вернёт значение dunder-метода `__str__` для каждого объекта:

```json
{
  "album_name": "Things We Lost In The Fire",
  "artist": "Low",
  "tracks": [
    "1: Sunflower",
    "2: Whitetail",
    "3: Dinosaur Act"
  ]
}
```

### PrimaryKeyRelatedField()

```python
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.PrimaryKeyRelatedField(many=True, read_only=True)

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

Вернёт `id`:

```json
{
  "album_name": "Undun",
  "artist": "The Roots",
  "tracks": [
    89,
    90,
    91
  ]
}
```

### HyperlinkedRelatedField()

```python
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.HyperlinkedRelatedField(
        many=True,
        read_only=True,
        view_name='track-detail'
    )

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

Вернёт ссылку на обработку объекта. О том, как работает эта магия, поговорим на следующем занятии.

```json
{
  "album_name": "Graceland",
  "artist": "Paul Simon",
  "tracks": [
    "http://www.example.com/api/tracks/45/",
    "http://www.example.com/api/tracks/46/",
    "http://www.example.com/api/tracks/47/"
  ]
}
```

### SlugRelatedField()

```python
class AlbumSerializer(serializers.ModelSerializer):
    tracks = serializers.SlugRelatedField(
        many=True,
        read_only=True,
        slug_field='title'
    )

    class Meta:
        model = Album
        fields = ['album_name', 'artist', 'tracks']
```

Вернёт то, что указано в атрибуте `slug_field`.

```json
{
    "album_name": "Dear John",
    "artist": "Loney Dear",
    "tracks": [
        "Airport Surroundings",
        "Everything Turns to You",
        "I Was Only Going Out"
    ]
}
```

И другие. О том, как сделать записываемые вложенные сериалайзеры и многое другое, читайте в документации.

## Немного забегая вперёд

Далее мы будем подробно рассматривать все особенности DRF и как превратить код в API, но в данный момент самым важным
для нас является то, что DRF предоставляет для нас полный функционал работы с API, самый простой пример использования
API выглядит так:

```python
from django.conf.urls import url, include
from django.contrib.auth.models import User
from rest_framework import routers, serializers, viewsets


# Serializers define the API representation.
class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'is_staff']


# ViewSets define the view behavior.
class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer


# Routers provide an easy way of automatically determining the URL conf.
router = routers.DefaultRouter()
router.register(r'users', UserViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    url(r'^', include(router.urls)),
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```


# @api_view, APIView, ViewSets, Pagination, Routers

Все мы помним, что веб в первую очередь - это Request-Response система.

## Request

Что нового в request.

Дока [тут](https://www.django-rest-framework.org/api-guide/requests/)

Два новых парамера `.data` и `.query_string`

`.data` - данные, если запрос POST, PUT или PATCH, аналог `request.POST` или `request.FILES`

`.query_string` - данные, если запрос GET, аналог `request.GET`

И параметры `.auth` и `.authenticateб`, которые мы рассмотрим на следующей лекции. Она целиком про авторизацию и
`permissions` (доступы).

## Response

Дока [тут](https://www.django-rest-framework.org/api-guide/responses/)

В отличие от классической Django, ответом в REST системе будет обычный HTTP-ответ, содержащий набор данных, чаще
всего JSON (но бывает и нет).

Классическая Django тоже может возвращать HTTP-ответ и быть обработчиком REST архитектуры, но существующий пакет
сильно упрощает эти процессы.

Для обработки такого ответа есть специальный объект:

```Response(data, status=None, template_name=None, headers=None, content_type=None)```

где `data` - данные,

`status` - код ответа (200, 404, 503),

`template_name` - возможность указать темплейт, если необходимо вернуть страницу, а не просто набор данных,

`headers` и `content_type` - заголовки и тип содержимого запроса.

## Настройка для получения JSON

В современной версии пакета DRF по умолчанию не указан параметр для получения ответа в формате JSON. Это нужно указать
явно, для этого в `settings.py` необходимо добавить:

```python
REST_FRAMEWORK = {
    'DEFAULT_RENDERER_CLASSES': (
        'rest_framework.renderers.JSONRenderer',
    )
}
```

## @api_view

Дока [тут](https://www.django-rest-framework.org/api-guide/views/#api_view)

Для описания endpoint функционально нужно указать декоратор `api_view` и методы, которые он может принимать. Возвращает
всё также объект ответа.

```python
from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer


@api_view(['GET', 'POST'])
def snippet_list(request):
    """
    List all code snippets, or create a new snippet.
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return Response(serializer.data)

    elif request.method == 'POST':
        serializer = SnippetSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

Обратите внимание, в пакете REST фреймворка сразу есть заготовленные объекты статуса для ответа.

Если попытаться получить доступ методом, который не разрешен, запрос будет отклонён с ответом `405 Method not allowed`

Для передачи параметров используются аргументы функции. (Очень похоже на обычную Django вью)

```python
@api_view(['GET', 'PUT', 'DELETE'])
def snippet_detail(request, pk):
    """
    Retrieve, update or delete a code snippet.
    """
    try:
        snippet = Snippet.objects.get(pk=pk)
    except Snippet.DoesNotExist:
        return Response(status=status.HTTP_404_NOT_FOUND)

    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)

    elif request.method == 'PUT':
        serializer = SnippetSerializer(snippet, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    elif request.method == 'DELETE':
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

URLs для таких методов описываются точно так же как и для стандартной Django вью.

```python
from snippets.view import snippet_list, snippet_detail

urlpatterns = [
    path('snippets/', snippet_list),
    path('snippets/<int:pk>/', snippet_detail),
]
```

Ответ на GET-запрос в этому случае будет выглядеть так:

```json
[
  {
    "id": 1,
    "title": "",
    "code": "foo = \"bar\"\n",
    "linenos": false,
    "language": "python",
    "style": "friendly"
  },
  {
    "id": 2,
    "title": "",
    "code": "print(\"hello, world\")\n",
    "linenos": false,
    "language": "python",
    "style": "friendly"
  }
]
```

Поля будут зависеть от модели и сериалайзера соответственно.

Ответ на POST запрос (создание объекта):

```json
http --form POST http://127.0.0.1:8000/snippets/ code="print(123)"

{
"id": 3,
"title": "",
"code": "print(123)",
"linenos": false,
"language": "python",
"style": "friendly"
}
```

## View

Знакомимся с самым подробным сайтом по DRF классам [тут](http://www.cdrf.co/)

## APIView

Дока [тут](https://www.django-rest-framework.org/api-guide/views/#class-based-views)

Также мы можем описать это же через Class-Based View, для этого нам нужно наследоваться от APIView:

```python
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status


class SnippetList(APIView):
    """
    List all snippets, or create a new snippet.
    """

    def get(self, request, format=None):
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return Response(serializer.data)

    def post(self, request, format=None):
        serializer = SnippetSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
```

Вынесем получение объекта в отдельный метод:

```python
class SnippetDetail(APIView):
    """
    Retrieve, update or delete a snippet instance.
    """

    def get_object(self, pk):
        try:
            return Snippet.objects.get(pk=pk)
        except Snippet.DoesNotExist:
            raise Http404

    def get(self, request, pk, format=None):
        snippet = self.get_object(pk)
        serializer = SnippetSerializer(snippet)
        return Response(serializer.data)

    def put(self, request, pk, format=None):
        snippet = self.get_object(pk)
        serializer = SnippetSerializer(snippet, data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data)
        return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

    def delete(self, request, pk, format=None):
        snippet = self.get_object(pk)
        snippet.delete()
        return Response(status=status.HTTP_204_NO_CONTENT)
```

URLs описываются так же, как и для Django Class-Based View:

```python
urlpatterns = [
    path('snippets/', SnippetList.as_view()),
    path('snippets/<int:pk>/', SnippetDetail.as_view()),
]
```

## GenericView

По аналогии с классической Django существуют заранее описанные CRUD действия.

Как это работает?

Существует класс `GenericAPIView`, который наследуется от обычного `APIView`.

В нём описаны такие поля как:

- `queryset` хранит кверисет;

- `serializer_class` хранит сериалайзер;

- `lookup_field = 'pk'` - название атрибута в модели, который будет отвечать за PK;

- `lookup_url_kwarg = None` - название атрибута в запросе, который будет отвечать за `pk`;

- `filter_backends = api_settings.DEFAULT_FILTER_BACKENDS` - фильтры запросов;

- `pagination_class = api_settings.DEFAULT_PAGINATION_CLASS` - пагинация запросов.

И методы:

- `get_queryset` - получение кверисета;

- `get_object` - получение одного объекта;

- `get_serializer` - получение объекта сериалайзера;

- `get_serializer_class` - получение класса сериалайзера;

- `get_serializer_context` - получить контекст сериалайзера;

- `filter_queryset` - отфильтровать кверисет;

- `paginator` - объект пагинации;

- `paginate_queryset` - пагинировать кверисет;

- `get_paginated_response` - получить пагинированый ответ.

*Такой класс не работает самостоятельно, только вместе с определёнными миксинами*

### Миксины

В DRF существует 5 миксинов:

```python
class CreateModelMixin(object):
    """
    Create a model instance.
    """

    def create(self, request, *args, **kwargs):
        serializer = self.get_serializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        self.perform_create(serializer)
        headers = self.get_success_headers(serializer.data)
        return Response(serializer.data, status=status.HTTP_201_CREATED, headers=headers)

    def perform_create(self, serializer):
        serializer.save()

    def get_success_headers(self, data):
        try:
            return {'Location': str(data[api_settings.URL_FIELD_NAME])}
        except (TypeError, KeyError):
            return {}
```

Рассмотрим подробнее.

Это миксин, и без сторонних классов этот функционал работать не будет.

При вызове метода `create()` мы предполагаем, что у нас был request.

Вызываем метод `get_serializer()` из класса `GenericAPIView` для получения объекта сериалайзера, обратите внимание, что
данные передаются через атрибут `data`, так как они получены от пользователя. Проверяем данные на валидность (обратите
внимание на атрибут `raise_exception`, если данные будут не валидны, код сразу вылетит в traceback, а значит нам не 
нужно отдельно прописывать действия при не валидном сериалайзере), вызываем метод `perform_create`, который просто 
сохраняет сериалайзер (вызывает `create` или `update` в зависимости от данных), получает хедеры, и возвращает response 
с 201 кодом, создание успешно.

По аналогии мы можем рассмотреть остальные миксины.

```python
class RetrieveModelMixin(object):
    """
    Retrieve a model instance.
    """

    def retrieve(self, request, *args, **kwargs):
        instance = self.get_object()
        serializer = self.get_serializer(instance)
        return Response(serializer.data)
```

Обратите внимание, метод называется `retrieve()` и внутри вызывает метод `get_object()`, - это миксин одиночного объекта

```python
class UpdateModelMixin(object):
    """
    Update a model instance.
    """

    def update(self, request, *args, **kwargs):
        partial = kwargs.pop('partial', False)
        instance = self.get_object()
        serializer = self.get_serializer(instance, data=request.data, partial=partial)
        serializer.is_valid(raise_exception=True)
        self.perform_update(serializer)

        if getattr(instance, '_prefetched_objects_cache', None):
            # If 'prefetch_related' has been applied to a queryset, we need to
            # forcibly invalidate the prefetch cache on the instance.
            instance._prefetched_objects_cache = {}

        return Response(serializer.data)

    def perform_update(self, serializer):
        serializer.save()

    def partial_update(self, request, *args, **kwargs):
        kwargs['partial'] = True
        return self.update(request, *args, **kwargs)
```

Методы `update()`, `partial_update()`, `perform_update()` нужны для обновления объекта, обратите внимание на атрибут 
`partial`. Помните разницу между PUT и PATCH?

```python
class DestroyModelMixin(object):
    """
    Destroy a model instance.
    """

    def destroy(self, request, *args, **kwargs):
        instance = self.get_object()
        self.perform_destroy(instance)
        return Response(status=status.HTTP_204_NO_CONTENT)

    def perform_destroy(self, instance):
        instance.delete()
```

Аналогично для удаления методы `destroy()`, `perform_destroy()`.

```python
class ListModelMixin(object):
    """
    List a queryset.
    """

    def list(self, request, *args, **kwargs):
        queryset = self.filter_queryset(self.get_queryset())

        page = self.paginate_queryset(queryset)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)

        serializer = self.get_serializer(queryset, many=True)
        return Response(serializer.data)
```

Метод `list()` получает кверисет, дальше пытается его пагинировать, если получается, возвращает страницу, если нет - 
целый ответ.

*Важно!* Ни в одном из миксинов не было методов `get()`,`post()`,`patch()`,`put()` или `delete()`, почему?

Потому что их вызов перенесен в дополнительные классы.

### Generic классы

Вот так выглядят классы, которые уже можно использовать. Как это работает? Эти классы наследуют логику работы с данными
из необходимого миксина, общие методы, которые актуальны для любого CRUD действия из `GenericAPIView` дальше описываем
методы тех видов запросов, которые мы хотим обрабатывать, в которых просто вызываем необходимый метод из миксина.

**Переписываются методы `create()`, `destroy()` и т. д., а не `get()`, `post()`!**

```python
class CreateAPIView(mixins.CreateModelMixin,
                    GenericAPIView):
    """
    Concrete view for creating a model instance.
    """

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)

```

```python
class ListAPIView(mixins.ListModelMixin,
                  GenericAPIView):
    """
    Concrete view for listing a queryset.
    """

    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
```

```python
class RetrieveAPIView(mixins.RetrieveModelMixin,
                      GenericAPIView):
    """
    Concrete view for retrieving a model instance.
    """

    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)
```

```python
class DestroyAPIView(mixins.DestroyModelMixin,
                     GenericAPIView):
    """
    Concrete view for deleting a model instance.
    """

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

```python
class UpdateAPIView(mixins.UpdateModelMixin,
                    GenericAPIView):
    """
    Concrete view for updating a model instance.
    """

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.partial_update(request, *args, **kwargs)
```

```python
class ListCreateAPIView(mixins.ListModelMixin,
                        mixins.CreateModelMixin,
                        GenericAPIView):
    """
    Concrete view for listing a queryset or creating a model instance.
    """

    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)

    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)
```

```python
class RetrieveUpdateAPIView(mixins.RetrieveModelMixin,
                            mixins.UpdateModelMixin,
                            GenericAPIView):
    """
    Concrete view for retrieving, updating a model instance.
    """

    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.partial_update(request, *args, **kwargs)
```

```python
class RetrieveDestroyAPIView(mixins.RetrieveModelMixin,
                             mixins.DestroyModelMixin,
                             GenericAPIView):
    """
    Concrete view for retrieving or deleting a model instance.
    """

    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

```python
class RetrieveUpdateDestroyAPIView(mixins.RetrieveModelMixin,
                                   mixins.UpdateModelMixin,
                                   mixins.DestroyModelMixin,
                                   GenericAPIView):
    """
    Concrete view for retrieving, updating or deleting a model instance.
    """

    def get(self, request, *args, **kwargs):
        return self.retrieve(request, *args, **kwargs)

    def put(self, request, *args, **kwargs):
        return self.update(request, *args, **kwargs)

    def patch(self, request, *args, **kwargs):
        return self.partial_update(request, *args, **kwargs)

    def delete(self, request, *args, **kwargs):
        return self.destroy(request, *args, **kwargs)
```

Допустим, мы хотим описать класс при GET запросе получение списка комментариев, в которых есть буква `w`, если у нас уже
есть сериалайзер и модель, а при POST создание комментария.

```python
class CommentListView(ListCreateAPIView):
    queryset = Comment.objects.all()
    serializer_class = CommentSerializer

    def get_queryset(self):
        return super().get_queryset().filter(text__icontains='w')
```

Всё, этого достаточно.

## ViewSet

Дока [тут](https://www.django-rest-framework.org/api-guide/viewsets/)

Классы, которые отвечают за поведение нескольких запросов, и отличаются друг от друга только методом, называются
`ViewSet`.

Они на самом деле описывают методы, для получения списка действий (list, retrieve, и т. д.), и преобразования их в URLs 
(об этом дальше).

Например:

```python
from django.contrib.auth.models import User
from django.shortcuts import get_object_or_404
from myapps.serializers import UserSerializer
from rest_framework import viewsets
from rest_framework.response import Response


class UserViewSet(viewsets.ViewSet):
    """
    A simple ViewSet for listing or retrieving users.
    """

    def list(self, request):
        queryset = User.objects.all()
        serializer = UserSerializer(queryset, many=True)
        return Response(serializer.data)

    def retrieve(self, request, pk=None):
        queryset = User.objects.all()
        user = get_object_or_404(queryset, pk=pk)
        serializer = UserSerializer(user)
        return Response(serializer.data)
```

Разные действия при наличии и отсутствии `PK`, при `GET` запросе.

Для описания URLs можно использовать разное описание:

```python
user_list = UserViewSet.as_view({'get': 'list'})
user_detail = UserViewSet.as_view({'get': 'retrieve'})
```

Хоть так никто и не делает, об этом дальше.

## ModelViewSet и ReadOnlyModelViewSet

Дока [тут](https://www.django-rest-framework.org/api-guide/viewsets/#modelviewset)

Объединяем всё, что мы уже знаем.

И получаем класс `ModelViewSet`, он наследуется от `GenericViewSet` (`ViewSet` + `GenericAPIView`) и всех 5 миксинов, а
значит там описаны методы `list()`, `retrieve()`, `create()`, `update()`, `destroy()`, `perform_create()`,
`perform_update()` и т. д.

А значит мы можем описать сущность, которая принимает модель и сериалайзер, и уже может принимать любые типы запросов и
выполнять любые CRUD действия. Мы можем их переопределить, или дописать еще экшенов, всё что нам может быть необходимо
уже есть.

Пример:

```python
class AccountViewSet(viewsets.ModelViewSet):
    """
    A simple ViewSet for viewing and editing accounts.
    """
    queryset = Account.objects.all()
    serializer_class = AccountSerializer
    permission_classes = [IsAccountAdminOrReadOnly]
```

Или если необходим такой же вьюсет только для получения объектов, то:

```python
class AccountViewSet(viewsets.ReadOnlyModelViewSet):
    """
    A simple ViewSet for viewing accounts.
    """
    queryset = Account.objects.all()
    serializer_class = AccountSerializer
```

На самом деле, можно собрать такой же вьюсет для любых действий, добавляя и убирая миксины.

Например:

```python
from rest_framework import mixins


class CreateListRetrieveViewSet(mixins.CreateModelMixin,
                                mixins.ListModelMixin,
                                mixins.RetrieveModelMixin,
                                viewsets.GenericViewSet):
    """
    A viewset that provides `retrieve`, `create`, and `list` actions.

    To use it, override the class and set the `.queryset` and
    `.serializer_class` attributes.
    """
    pass
```

Чаще всего используются обычные `ModelViewSet`.

### Пагинация

Дока [тут](https://www.django-rest-framework.org/api-guide/pagination/)

Как мы помним, для действия `list` используется пагинация. Как это работает?

Если у нас нет необходимости настраивать все вьюсеты отдельно, то мы можем указать такую настройку в `settings.py`:

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.LimitOffsetPagination',
    'PAGE_SIZE': 100
}
```

Там мы можем указать тип класса пагинации и размер одной страницы, и все наши запросы уже будут пагинированы.

Также мы можем создать классы пагинаторов, основываясь на нашей необходимости.

```python
class LargeResultsSetPagination(PageNumberPagination):
    page_size = 1000
    page_size_query_param = 'page_size'
    max_page_size = 10000


class StandardResultsSetPagination(PageNumberPagination):
    page_size = 100
    page_size_query_param = 'page_size'
    max_page_size = 1000
```

Если нужно указать пагинатор у конкретного вьюсета, то можно это сделать прямо в атрибутах.

```python
class BillingRecordsView(generics.ListAPIView):
    queryset = Billing.objects.all()
    serializer_class = BillingRecordsSerializer
    pagination_class = LargeResultsSetPagination
```

## Декоратор @action

Дока [тут](https://www.django-rest-framework.org/api-guide/viewsets/#marking-extra-actions-for-routing)

Что делать, если вам нужно дополнительное действие, связанное с деталями вашей вью, но ни один из крудов не походит? Тут
можно использовать декоратор `@action`, чтобы описать новое действие в этом же вьюсете.

```python
from django.contrib.auth.models import User
from rest_framework import status, viewsets
from rest_framework.decorators import action
from rest_framework.response import Response
from myapp.serializers import UserSerializer, PasswordSerializer


class UserViewSet(viewsets.ModelViewSet):
    """
    A viewset that provides the standard actions
    """
    queryset = User.objects.all()
    serializer_class = UserSerializer

    @action(detail=True, methods=['post'])
    def set_password(self, request, pk=None):
        user = self.get_object()
        serializer = PasswordSerializer(data=request.data)
        if serializer.is_valid():
            user.set_password(serializer.data['password'])
            user.save()
            return Response({'status': 'password set'})
        else:
            return Response(serializer.errors,
                            status=status.HTTP_400_BAD_REQUEST)

    @action(detail=False)
    def recent_users(self, request):
        recent_users = self.get_queryset().order_by('-last_login')

        page = self.paginate_queryset(recent_users)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)

        serializer = self.get_serializer(recent_users, many=True)
        return Response(serializer.data)
```

Принимает два основных параметра: `detail` описывает должен ли этот action принимать PK (действие над всеми объектами
или над одним конкретным), и `methods` - список HTTP методов, на которые должен срабатывать `action`.

Есть и другие, например, классы permissions или имя.

## Роутеры

Дока [тут](https://www.django-rest-framework.org/api-guide/routers/)

Роуте - это автоматический генератор URLs для вьюсетов.

```python
from rest_framework import routers

router = routers.SimpleRouter()
router.register(r'users', UserViewSet)
router.register(r'accounts', AccountViewSet)
urlpatterns = router.urls
```

В методе `register` принимает два параметра, на каком слове основывать URLs и для какого вьюсета.

Если у вьюсета нет параметра `queryset`, то нужно указать поле `basename`, если нет, то автоматически будет использовано
имя модели маленькими буквами.

URLs будут сгенерированы автоматически, и им будут автоматически присвоены имена:

```python
URL pattern: ^users/$ Name: 'user-list'
URL pattern: ^users/{pk}/$ Name: 'user-detail'
URL pattern: ^accounts/$ Name: 'account-list'
URL pattern: ^accounts/{pk}/$ Name: 'account-detail'
```

Чаще всего роутеры к URLs добавляются вот такими способами:

```python
urlpatterns = [
    path('forgot-password', ForgotPasswordFormView.as_view()),
    path('api/', include(router.urls)),
]
```

### Роутинг экстра экшенов

Допустим, есть такой экстра экшен:

```python
from rest_framework.decorators import action


class UserViewSet(ModelViewSet):
    ...

    @action(methods=['post'], detail=True)
    def set_password(self, request, pk=None):
        ...
```

Роутер автоматически сгенерирует URL `^users/{pk}/set_password/$` и имя `user-set-password`.

Класс `SimpleRouter` может принимать параметр `trailing_slash=False` True или False, по дефолту True, поэтому все API,
должны принимать URLs заканчивающиеся на `/`, если указать явно, то будет принимать всё без `/`.


# REST аутентификация. Авторизация. Permissions. Фильтрация.

![](https://encrypted-tbn0.gstatic.com/images?q=tbn%3AANd9GcQ_cLOesie903fmPPbl2YbqawKsycva_owf6Q&usqp=CAU)

## Аутентификация и её виды

Мы с вами разобрали аутентификацию для работы классического веб-приложения, на самом деле, это был лишь один из видов
существующих аутентификаций, давайте рассмотрим разные.

### Аутентификация сессией

Основана на данных сессии, которые мы уже рассматривали. Авторизация происходит один раз, после чего информация о
пользователе хранится в "куках" и передаётся при каждом запросе.

В чём недостатки такого подхода для REST API?

Во-первых, для того чтобы выполнять любые небезопасные методы `POST`, `PUT`, `PATCH`, `DELETE` необходимо использовать 
CSRF Token, а это значит, что для выполнения таких запросов необходимо каждый раз делать дополнительный запрос для
получения токена.

Во вторых такой подход подразумевает, что сервер хранит информацию о сессиях, такой подход не будет RESTful.

### Базовая аутентификация

Аутентификация основанная на том, что в каждом запросе в хедере запроса будет передаваться логин и пароль необходимого
пользователя. Чаще всего для использования такого вида аутентификации в запрос добавляется хедер `Authorization` со
значением, состоящим из слова `Basic` и кодированного при помощи base64 сообщения вида `username:password` например:

`Authorization`: `Basic bXl1c2VyOm15cGFzc3dvcmQ=` для `username` - `myuser`, `password` - `mypassword`

Такая авторизация требует подключения по `https`, так как при обычном `http` запрос легко будет перехватить и
посмотреть данные авторизации.

### Аутентификация по токену

Базовая аутентификация имеет большое количество плюсов перед сессионной, как минимум отсутствие необходимости делать
дополнительные запросы, но каждый раз передавать логин и пароль это не самый удобный с точки зрения безопасности способ
передачи данных.

Поэтому самым частым видом авторизации является авторизация по токену. Что это значит?

**Токен** - это специальный вычисляемый набор символов, уникальный для каждого пользователя.

Токен может быть как постоянным (практически никогда не используется), так и временным, может перегенерироваться по
времени, так и по запросу.

Алгоритм генерации самого токена тоже может быть практически любым (Чаще всего просто генерация большой случайной hex 
(шестнадцатеричной) строки), как и данные, на которых он основан (при случайном токен входных данных нет, но может быть 
основан на каких-либо личных данных, на метках времени и т. д.)

### Внешняя аутентификация

Благодаря механизму токенов, за авторизацию может отвечать вообще не ваш сервер. Допустим, если взять классическую
авторизацию через соцсети, то генератором токена является сама соцсеть, мы лишь предоставляем данные для авторизации
соцсети. В ответ получаем токен, при этом мы понятия не имеем, как именно его генерирует условный фейсбук, но всегда
можно убедится в его правильности, обратившись к API соцсети.

По такому же принципу сервером авторизации может быть практически любой внешний сервер, с которым есть предварительная
договорённость. Допустим, вы работаете с командой, которая его разрабатывает, и можете узнать, как этим пользоваться. 
Для открытых соцсетей обычно есть документация по использованию их API, где подробно написано, как пользоваться их
авторизацией. Также для таких механизмов существует большое количество уже написанных packages.

## Реализация и использование в Django REST Framework

Из-за CSRF токенов авторизация через сессию практически не используется, поэтому мы не будем подробно её рассматривать

### Basic Аутентификация

Чтобы использовать Basic аутентификацию, достаточно добавить в настройку `REST_FRAMEWORK`, в `settings.py`:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.BasicAuthentication',
    ]
}
```

Если нам необходимо использовать несколько аутентификаций, мы можем указать их в списке, например:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        'rest_framework.authentication.BasicAuthentication',
        'rest_framework.authentication.SessionAuthentication',
    ]
}
```

Этого достаточно, чтобы при любом запросе сначала проверялся хедер авторизации, и в случае правильных логина и пароля
пользователь добавлялся в реквест.

Если необходимо добавить классы авторизация прям во вью, можно указать их через аттрибут `authentication_classes` для
Class-Based View и такой же декоратор для функциональной вью.

```python
from rest_framework.authentication import SessionAuthentication, BasicAuthentication
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response
from rest_framework.views import APIView


class ExampleView(APIView):
    authentication_classes = [SessionAuthentication, BasicAuthentication]

    def get(self, request, format=None):
        content = {
            'user': unicode(request.user),  # `django.contrib.auth.User` instance.
            'auth': unicode(request.auth),  # None
        }
        return Response(content)
```

```python
@api_view(['GET'])
@authentication_classes([SessionAuthentication, BasicAuthentication])
@permission_classes([IsAuthenticated])
def example_view(request, format=None):
    content = {
        'user': unicode(request.user),  # `django.contrib.auth.User` instance.
        'auth': unicode(request.auth),  # None
    }
    return Response(content)
```

### Авторизация по токену

Если необходимо использовать токен авторизацию, то DRF предоставляет нам такой функционал "из коробки", для этого нужно
добавить `rest_framework.authtoken` в `INSTALLED_APPS`

```python
INSTALLED_APPS = [
    ...
    'rest_framework.authtoken'
]
```

И указать необходимую авторизацию в `settings.py`:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': [
        ...
        'rest_framework.authentication.TokenAuthentication',
    ]
}
```

После этого обязательно нужно провести миграции этого приложения `python manage.py migrate`

Чтобы создать токены для уже существующих юзеров, нужно сделать это вручную (или написать дата миграцию).

```python
from rest_framework.authtoken.models import Token

token = Token.objects.create(user=...)
print(token.key)
```

После этого можно использовать авторизацию токеном:

```Authorization: Token 9944b09199c62bcf9418ad846dd0e4bbdfc6ee4b```

#### Генерация токенов

Чаще всего генерацию токенов "вешают" на сигналы:

```python
from django.conf import settings
from django.db.models.signals import post_save
from django.dispatch import receiver
from rest_framework.authtoken.models import Token


@receiver(post_save, sender=settings.AUTH_USER_MODEL)
def create_auth_token(sender, instance=None, created=False, **kwargs):
    if created:
        Token.objects.create(user=instance)
```

#### Получение токена

Для получения токена можно использовать стандартную вью, для этого нужно добавить в URLs `obtain_auth_token`:

```python
from rest_framework.authtoken import views

urlpatterns += [
    path('api-token-auth/', views.obtain_auth_token)
]
```

Если необходимо изменить логику получения токена, то это можно сделать, отнаследовавшись
от `from rest_framework.authtoken.views import ObtainAuthToken`:

```python
from rest_framework.authtoken.views import ObtainAuthToken
from rest_framework.authtoken.models import Token
from rest_framework.response import Response


class CustomAuthToken(ObtainAuthToken):

    def post(self, request, *args, **kwargs):
        serializer = self.serializer_class(data=request.data,
                                           context={'request': request})
        serializer.is_valid(raise_exception=True)
        user = serializer.validated_data['user']
        token, created = Token.objects.get_or_create(user=user)
        return Response({
            'token': token.key,
            'user_id': user.pk,
            'email': user.email
        })
```

Не забыв заменить URLs:

```python
urlpatterns += [
    path('api-token-auth/', CustomAuthToken.as_view())
]
```

### Кастомная авторизация

Кроме своего токена и своего способа его получения, можно также расписать и свою собственную авторизацию, для этого
нужно отнаследоваться от базовой и описать нужные методы:

```python
from django.contrib.auth.models import User
from rest_framework import authentication
from rest_framework import exceptions


class ExampleAuthentication(authentication.BaseAuthentication):
    def authenticate(self, request):
        username = request.META.get('HTTP_X_USERNAME')
        if not username:
            return None

        try:
            user = User.objects.get(username=username)
        except User.DoesNotExist:
            raise exceptions.AuthenticationFailed('No such user')

        return (user, None)
```

### Manage-команда drf_create_token

```python manage.py drf_create_token <username>```

Принимает параметр `username` и генерирует токен для такого юзера, если необходимо, то можно перегенерировать при помощи
флага `-r`

```
python manage.py drf_create_token -r <username>
```

### Немного о реальности

На практике практически всегда необходимо переписать токен под свои задачи, как минимум ограничить его время для жизни и
сделать перегенерацию по истечении времени жизни, сделаем это как практику на этом занятии.

### Внешние сервисы

По сути, каждый отдельный сервис имеет свою логику, чаще всего у нас будут специальные пакеты для использования таких
аутентификаций, а если нет, то их всегда можно написать. :)

### Авторизация для тестирования через браузер

В REST фреймворк встроена возможность тестировать API через браузер, используя сессионную авторизацию. Для этого
достаточно добавить встроенные URLs и перейти по этому адресу, после этого по вашим API URLs вы будете переходить как
уже авторизированный пользователь:

```python
urlpatterns += [
    path('api-auth/', include('rest_framework.urls')),
]
```

## Permissions

Они же права доступа.

Задать разрешения можно на уровне проекта и на уровне ресурса.

Чтобы задать на уровне проекта, в `settings.py` необходимо добавить:

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated',
    ]
}
```

Для описания на уровне объектов используется аргумент `permission_classes`:

```python
from rest_framework import permissions


class ExampleModelViewSet(ModelViewSet):
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
```

Существует достаточно много заготовленных пермишенов.

```
AllowAny - можно всем
IsAuthenticated - только авторизированным пользователям
IsAdminUser - только администраторам
IsAuthenticatedOrReadOnly - залогиненым или только на чтение
```

Все они изначально наследуются от `rest_framework.permissons.BasePermission`.

Но если нам нужны кастомные, то мы можем создать их, отнаследовавшись от `permissions.BasePermission` и переписав один 
или оба метода `has_permisson()` и `has_object_permission()`

Например, владельцу можно выполнять любые действия, а остальным только чтение объекта:

```python
from rest_framework import permissions


class IsOwnerOrReadOnly(permissions.BasePermission):
    """
    Custom permission to allow only owners of an object to edit it.
    """

    def has_object_permission(self, request, view, obj):
        # Read permissions are allowed to any request,
        # so we'll always allow GET, HEAD or OPTIONS requests.
        if request.method in permissions.SAFE_METHODS:
            return True

        # Write permissions are only allowed to the owner of the snippet.
        return obj.owner == request.user

    def has_permission(self, request, view):
        return True
```

`has_permission()` - отвечает за доступ к спискам объектов
`has_object_permission()` - отвечает за доступ к конкретному объекту

Пермишены можно указывать через запятую, если их несколько:

```python
permission_classes = [permissions.IsAuthenticatedOrReadOnly,
                      IsOwnerOrReadOnly]
```

Если у вас нет доступов, вы получите вот такой ответ:

```
{
    "detail": "Authentication credentials were not provided."
}
```

### Пример использования авторизации в ресурсах

```python
class SomeModelViewSet(ModelViewSet):
    serializer_class = SomeSerializer
    queryset = SomeModel.objects.all()

    def perform_create(self, serializer):
        serializer.save(user=self.request.user)
```

Таким образом, мы можем добавлять объект юзера при сохранении нашего сериалайзера.


## ДЗ

- По сериалайзерам
1. Создать сериалайзер для обработки данных из 1 задания из лекции про формы (напишите форму, в которой можно указать
   имя, пол, возраст и уровень владения английским (выпадающим списком), если введенные данные - это парень старше 20-и 
   (включительно) и уровнем английского выше B2 или девушка старше 22-х и уровнем выше B1, то перейти на страницу,
   где будет написано, что вы нам подходите или что не подходите, соответственно.)

   1.1 Зайти в shell. Заполнить сериалайзер через `data=` данными. 
   Убедиться, что валидация работает в соответствии с требованиями.

2. Создать сериалайзеры для Юзера, Покупки, Товара.

   2.1 Создать объект Юзера, Товара, Покупки, связанных между собой (данные передать через `data=`)

   2.2 Получить объекты из базы, передать в сериалайзер без `data=`, посмотреть, что у них хранится в атрибуте `.data`.

3. Написать сериалайзер для Покупки (новый), который будет хранить вложенный сериалайзер Юзера.
   
   3.1 Получить данные любого товара вместе с данными о юзерa.

4. Написать сериалайзер для Юзера, который будет хранить все его Покупки и выдавать их списком из словарей. (`many=True`)

   4.1 Получить данные любого юзера.

5*. Дописать сериалайзеры из пунктов 4 и 5 так, чтобы можно было создавать объекты. (Это сложно.)

- По построению API

Создать две модели. Книга и Автор, связанные через ForeignKey, у автора есть имя и возраст. У книги есть название.

1) Полный CRUD для двух объектов, книга и автор, должны работать все виды запросов через `postman`.

2) При создании книги добавлять в конце названия символ "!".

3) Для метода `GET` книги, добавить опциональный параметр `author_age`, если он указан и там число, то отображать только
   книги, если возраст автора больше или равен указанного.

4) Добавить отдельный экшен, чтобы получать объект автора с полем `books`, в котором будут лежать `id` всех книг этого
   автора.

- По аутентификации

1. Пишем Basic Authentication.

2. Пишем Token Authentication.

3. Пишем viewsets для моделей из модуля и создаём 2 покупки и 1 возврат через postman.

