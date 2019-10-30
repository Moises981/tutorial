# Django URLs

첫 웹페이지를 만들어 보기로 해요: 여러분의 블로그를 위한 홈페이지요! 그 전에 먼저 장고 URL에 대해서 조금 알아봅시다.

## URL이란 무엇인가요?

URL은 웹 주소입니다. 웹사이트를 방문할 때마다 URL을 볼 수 있으며 브라우저의 주소창에 표시됩니다. (맞아요! `127.0.0.1:8000`가 URL입니다! 그리고 `https://djangogirls.org` 또한 URL이죠.)

![URL](images/url.png)

인터넷에 있는 모든 페이지들은 자신만의 URL을 가지고 있어야 해요. This way your application knows what it should show to a user who opens that URL. 장고 에서는 `URLconf` (URL configuration)라는걸 사용합니다. URLconf is a set of patterns that Django will try to match the requested URL to find the correct view.

## Django에서 URL은 어떻게 작동할까요?

코드 에디터에서 `mysite/urls.py`파일을 열면 아래와 같을 거에요. :

{% filename %}mysite/urls.py{% endfilename %}

```python
"""mysite URL Configuration

[...]
"""
from django.contrib import admin
from django.urls import path

urlpatterns = [
    path('admin/', admin.site.urls),
]
```

As you can see, Django has already put something here for us.

세 개의 따옴표들(`"""`, `'''`) 사이에 있는 줄들은 독스트링(docstring)이라 불려요. 파일의 최상단이나 클래스 혹은 메서드 위에 적어두면, 그것이 어떤 역할을 하는지 알려주는 역할을 합니다. 여기있는 내용은 실행되지 않아요.

이전 장에서 봤던 관리자 URL도 여기에 이미 있어요:

{% filename %}mysite/urls.py{% endfilename %}

```python
    path('admin/', admin.site.urls),
```

This line means that for every URL that starts with `admin/`, Django will find a corresponding *view*. In this case, we're including a lot of admin URLs so it isn't all packed into this small file – it's more readable and cleaner.

## Your first Django URL!

첫 번째 URL을 만들어 봅시다! 우리는 '<http://127.0.0.1:8000/>' 이 주소를 우리 블로그의 홈페이지 주소로 쓸 것이고, 이 주소로 들어가면 글 목록이 보이게 만들어 볼 거에요.

We also want to keep the `mysite/urls.py` file clean, so we will import URLs from our `blog` application to the main `mysite/urls.py` file.

먼저, `blog.urls`를 가져오는 행을 추가해 봅시다. `blog.urls`를 가져오려면, `include` 함수를 이용해야 합니다. 그 함수를 이용하려면, `from django.urls` 로 시작하는 행을 수정해야 할 거에요.

Your `mysite/urls.py` file should now look like this:

{% filename %}mysite/urls.py{% endfilename %}

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
]
```

Django will now redirect everything that comes into 'http://127.0.0.1:8000/' to `blog.urls` and looks for further instructions there.

## blog.urls

Create a new empty file named `urls.py` in the `blog` directory, and open it in the code editor. All right! Add these first two lines:

{% filename %}blog/urls.py{% endfilename %}

```python
from django.urls import path
from . import views
```

우리는 장고에서 제공되는 함수 `path`와 `blog` 애플리케이션에서 사용할 모든 `views`를 불러오고 있어요. (물론 아직 뷰를 하나도 안 만들었지만, 곧 만들거니 조금만 기다리세요!)

After that, we can add our first URL pattern:

{% filename %}blog/urls.py{% endfilename %}

```python
urlpatterns = [
    path('', views.post_list, name='post_list'),
]
```

이제 `post_list`라는 이름의 `view`가 루트 URL에 할당되었습니다. This URL pattern will match an empty string and the Django URL resolver will ignore the domain name (i.e., http://127.0.0.1:8000/) that prefixes the full URL path. 따라서, 이 패턴은 장고에게 누군가 웹사이트에 'http://127.0.0.1:8000/' 주소로 들어왔을 때 `views.post_list`를 보여주라고 말할 거에요.

The last part, `name='post_list'`, is the name of the URL that will be used to identify the view. This can be the same as the name of the view but it can also be something completely different. We will be using the named URLs later in the project, so it is important to name each URL in the app. We should also try to keep the names of URLs unique and easy to remember.

지금 http://127.0.0.1:8000를 접속 하려 하면 'web page not available(웹 페이지를 사용할 수 없습니다)'와 같은 메세지가 보일 겁니다. 이렇게 뜨는건 지금은 서버가(`runserver`라고 입력했던걸 기억하시나요?) 실행되고 있지 않기 때문이에요. 서버 콘솔을 보고 왜 그런지 이유를 찾아봅시다.

![Error](images/error1.png)

콘솔에서 에러가 발생했네요. 하지만 걱정하지 마세요. 에러는 해결할 방법을 알려준답니다. : __no attribute 'post_list'__ 라는 메시지가 보일 텐데요. 이것은 장고가 찾고 사용하려고하는 *뷰*가 아직 없다는 거에요. 이 단계에서 `/admin/`로도 접속되지 않을 거에요. 앞으로 고쳐볼 테니 걱정하지 마세요. 혹시 여러분이 다른 에러 메시지를 보게 된다면, 웹서버를 껐다 켜보세요. To do that, in the console window that is running the web server, stop it by pressing Ctrl+C (the Control and C keys together). On Windows, you might have to press Ctrl+Break. Then you need to restart the web server by running a `python manage.py runserver` command.

> If you want to know more about Django URLconfs, look at the official documentation: https://docs.djangoproject.com/en/2.2/topics/http/urls/