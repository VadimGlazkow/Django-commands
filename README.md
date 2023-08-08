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
        
# **Шаблоны и статические файлы**
### **Что такое шаблон и как его создать?**
1. В папке приложения создать папку `templates`;
2. В папке `templates` создать папку с названием приложения;
3. Уже в папке с названием приложения создавать `html` файлы;
4. В файле `views.py` в функции: `return render(request, 'horoscope/info_zodiac.html')`;






