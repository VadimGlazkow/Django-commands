# **Шпаргалка по Django**


# **Базовые команды в cmd**
* `dir`  — покажет файлы и папки текущей папки (аналог `ls` в `Linux` или `Git Bash`);
* `python -V`  — покажет текущую версию `python`;
* `where python` — покажет версию интерпретатора, которая будет использоваться при обращении к `python` в командной строке;

# **Настройка виртуального окружения**
1. Установка модуля: `pip install virtualenv` (пакет ставится на системный `python`);
2. Создание виртуального окружения: `python -m venv venv` (выполняется в папке текущего проекта);
3. Активация виртуального окружения (windows): `venv\Scripts\activate.bat`;
4. Установка пакетов в виртуальное окружение;
5. Деактивация виртуального окружения: `deactivate`;

# **Команды Django**

### **Создание django проекта**
* `django-admin` — показывает возможные команды `django`;
* `django-admin startproject <имя проекта>` — создает проект `django`
* `python manage.py runserver` — `django` запускает локальный веб-серсер;
* `Сtrl` `+` `С` — остановить локальный веб-сервер;
* `python manage.py startapp <имя приложения>` — создание приложения;


### **Создание проекта**
1. Создать `django` проект: `django-admin startproject <имя_проекта>`;

2. Создать виртуальное окружение;
3. Создать приложение: `python manage.py startapp <имя_приложения>`;
4. Добавить название приложения в файл: `/<имя_проекта>/settings.py` в переменную `INSTALLED_APPS`;
5. В приложении создать файл `urls.py`;
6. Работа в папке проекта в файле `urls.py`:
    1. Импортировать `include`: `from django.urls import path, include`;
    2. В список `urlpatterns` добавить: `path('<желаемый_url>/', include('<название_приложения>.urls'))`;
7. Работа с приложением в файле `urls.py`:
    1. Создать шаблон:
        ```py
        from django.urls import path
        from . import views

        urlpatterns = [
        ]
        ```
    2. Добавить в список `urlpatterns` -> `path`: `path({url}/', views.<название фукнции в файле views, соответсвующий данному url>)`
    3. Также можно добавить динамический `url`: `path(<тип_данных:название переменной>, ...)`;
    4. Можно создать псевдомин роутеру, добавив аргумент `name`;

8. В папке приложения в файле `views.py`:
    1. Создать функции, соответсвуйющий опеределенноему `url`. Первый обязательный аргумент: `request`, далее аргументы добавляются в зависимости от динамичности `url`;
    2. Cделать импорт: `from django.http import HttpResponse, HttpResponseNotFound`;
    3. Сделать импорт: `from django.http import HttpResponseRedirect, HttpResponsePermanentRedirect` или `from django.shortcuts import redirect`;
    4. Возвращение результата:
        1. Если все ок, то: `return HttpResponse()`;
        2. Если нужно вернуть сообщение с ошибкой, то `HttpResponseNotFound()`;
        3. Если нужно перенаправить то:
            1. Хард-кодом: 
                * `return HttpResponseRedirect(f'/horoscope/{name_zodiac}/')` или;
                * `return HttpResponsePermanentRedirect(f'/horoscope/{name_zodiac}/')` или;
                * `return redirect(f'/horoscope/{name_zodiac}/', permanent=..., kwargs=...)`;
            2. Рекомендуемым способом:
                ```py
                from django.urls import reverse
                redirect_url = reverse('<псевдоним_роутра>', args=[])
                return HttpResponseRedirect(redirect_url)
                ```
    5. Тестирование в файле `tests.py`:
        ```py
        from django.test import TestCase
    
    
        class TextHoroscope(TestCase):
            def test_index(self):
                response = self.client.get('/horoscope/')
                self.assertEqual(response.status_code, 200)
        
            def test_libra(self):
                response = self.client.get('/horoscope/libra/')
                self.assertEqual(response.status_code, 200)
                self.assertIn('Весы', response.content.decode())
        
            def test_libra_redirect(self):
                response = self.client.get('/horoscope/7/')
                self.assertEqual(response.status_code, 302)
                self.assertEqual(response.url, '/horoscope/libra/')
        ```
        
# **Шаблоны**
### **Что такое шаблон и как его создать?**
1. В папке приложения создать папку `templates`;
2. В папке `templates` создать папку с названием приложения;
3. Уже в папке с названием приложения создавать `html` файлы;
4. В файле `views.py` в функции: `return render(request, 'horoscope/info_zodiac.html')`;


# **Django template language**
### **Перечада аргументов:**
* Аргументы передаются в виде словаря третьим параметров в `render()` в ключ `context=`;

### **Использование аргументов**
* Переменные записываются в `{{<переменная>}}`;

### **Тег if**
```html
{% if условие %}
    <тело>
{% elif (условие) %}
    <тело>
{% else %}
    <тело>
{% endif %}
```

### **Тег for**
```html
{% for elem in elems %}
    <тело>
{% empty %}
    <тело>
{% endfor %}
```

### **Переменные тега for**
1. `forloop.counter` - номер текущей итерации цикла, начиная с 1.
2. `forloop.counter0` - номер текущей итерации цикла, начиная с 0.
3. `forloop.revcounter` - номер текущей итерации цикла, начиная с последнего элемента списка.
4. `forloop.revcounter0` - номер текущей итерации цикла, начиная с последнего элемента списка и начиная с 0.
5. `forloop.first` - True, если это первая итерация цикла, False в противном случае.
6. `forloop.last` - True, если это последняя итерация цикла, False в противном случае.
7. `forloop.parentloop` - доступ к переменным из внешнего цикла, если вложенный цикл используется в шаблоне.

### **Тег url**
* `{% url 'some-url-name' v1 v2 %}` - аналог `reverse()`;

### **Теги block и extends**
1. Создаем папку `templates` в директории проекта;
2. В этой папке создаем файл `base.html`;
3. в папке `<имя_проекта>` в файеле `settings.py` в переменной `TEMPLATES.DIRS` добавить `BASE_DIR / 'templates',`;
* тег `block` записывается так:
    ```html
    {% block <имя_блока> %}
    {% endblock %}
    ```
* тег `extends` выглядит так: `{% extends 'base.html' %}`;

### **Тег include**
1. В папке приложения `templates` создаем папку `includes` и в ней уже создаем файлм `html`;
2. В файл `html` папки `includes` создаем `html` код;
3. Для подключения кода вводим: `{% include 'includes/<имя_файла>.html' %}`;
4. Если ходит, чтобы это ткод не видел переменные, то: ``{% include 'includes/<имя_файла>.html' only %}``;
5. Можно записывать свои переменные: `{% include 'includes/navbar.html' with a=100 b=200 %}`;

# **Статические файлы**
1. строим структуру папок:
    ```
                                 ,> css
    `<папка_проета>` -> `static` -> js
                                 `> img
    ```
2. Вначале прописать: `{% load static %}`;
3. В теге `head` прописать `<link rel="stylesheet" href="{% static <имя_приложения>/css/<имя_файла>.css %}">` ;
4. Чтобы сработал `css` в глобальной папке, нужно в файле `settings.py` прописать:
    ```
    STATICFILES_DIRS = [
        BASE_DIR / 'static',
    ]
    ```
    
# **Базы данных, ORM, модели**
### **Создать БД**
* В файле `models.py` нужно создать модель (то есть класс, описывающий БД):
    ```py
    from django.db import models
    
    # Create your models here.
    
    
    class Movie(models.Model):
        name = models.CharField(max_length=40)
        rating = models.IntegerField(default=1)
    ```

* Создать инструкцию для БД: `python manage.py makemigrations`;
* Для применения миграции: `python manage.py migrate`;
* Откатить миграцию: `python3 manage.py migrate имя_приложения номер_миграции`;
* `python manage.py showmigrations` - показать созданные миграции;

### **Добавление записи в таюлицу**
1. Устарновить библиотеку: `pip install ipython` (для удобной консоли);
2. Установить библиотеку: `pip install django-extensions` (для вывода SQL кода);
3. Добавляем в файл `settings.py`: 
    ```py
    INSTALLED_APPS = (
        ...
        'django_extensions',
    )
    ```
4. Для работы в терминале: `python manage.py shell_plus --print-sql`;
5. Добавить элементы: `Movie(name='Matrix', rating=85).save()` или `Movie.objects.create(name='Matrix', rating=85)`;
6. Закрыть консоль - `exit()`;

### **Выборка записей из таблицы**
1. В файле `models.py` классе реализовать магический метод `__str__`;
2. В консоли можно работать с `<Название_модели>.objects.all()`;
3. Также можно обращаться к элементам строки:
    ```py
    elem = Movie.objects.all()[0]
    elem.name  # Avatar
    elem.rating  # 80
    ```

### **Изменение и удаление записей**
* Изменение:
    ```
    obj = Movie.objects.all()[3]
    obj.name = 'qwerty'
    obj.save()
    ```
* Удаление:
    ` Movie.objects.all()[3].delete()`

### **фильтры данных при помощи `get` и `filter`**
* `Movie.objects.filter(budget=1000)` - фильтр на равенство поля;
* `Movie.objects.filter(budget__gt=1000)` - (>) фильтр на поле больше значения (great then);
* `Movie.objects.filter(budget__lt=1000)` - (<) фильтр на поле меньше значения;
* `Movie.objects.filter(budget__gte=1000)`- (>=) фильтр на поле больше либо равно значения;
* `Movie.objects.filter(budget__lte=1000)` - (<=) фильтр на поле меньше либо равно значения;
* `Movie.objects.exclude(budget=1000)` - фильтр на поле не равно значению;
* `Movie.objects.filter(year__isnull=True)` - фильтр на поле пустое (False - не пустое);
* `Movie.objects.filter(year__isnull=True, name=’Avatar’)` - фильтр на два поля (AND);
* `Movie.objects.exclude(budget=1000).filter(name=’Avatar’)` - фильтр на два поля (AND);
* `Movie.objects.filter(name__contains=’Avatar’)` поле содержит значение, чувствителен к регистру (кроме SQLite, для него не чувствителен);
* `Movie.objects.filter(name__icontains=’Avatar’)` - поле содержит значение, не чувствителен к регистру;
* `Movie.objects.filter(name__startswith=’a’)` поле начинается с “a”, `endswith` - заканчивается;
* `Movie.objects.filter(id__in=[3,5,6])` выбираются все значения из списка;





