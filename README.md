# 中文：A django/react skeleton using Webpack and Heroku Django template.

### 怎样把Django后台跟React.js利用webpack合起来的网路应用一小部分。

## Features (功能)

## How-to （手册)

1. 把Django Heroku Template下载， 应该看一下not_README.md的教程。试一下： `python manage.py runserver`.

2. 没有问题的话，用`npm`撞上数据户的方法： `npm init --yes`

3. 然后，打这样： `npm install --save webpack
react
react-dom
react-router-dom
redux
react-redux
babel-core
babel-loader
babel-preset-react
babel-preset-es2015`

4. 数据户撞上的时候, 做成一张webpack.config.js, 好像这样：

```javascript
  // webpack.config.js
  path = require('path')

  module.exports = {
    context: __dirname,
    entry: './frontend/entry.jsx',
    output: {
      path: path.resolve(__dirname, 'frontend','static','js'),
      filename: 'bundle.js'
    },
    resolve: {
      extensions: ['.js', '.jsx', '*']
    },
    module: {
      loaders: [
        {
          test: /\.jsx?$/,
          exclude: /(node_modules)/,
          loader: 'babel-loader',
          query: {
            presets: ['es2015','react']
          }
        }
      ]
    },
    devtool: 'source-maps',
    watch: true
  }
```
那一些复数有点儿复杂， 就是告诉Django后台怎么找道React.js前台的资源。

5. 创一个文件夹叫 `frontend`, 在里面创一个文件夹叫 `static`, 再创两个文件叫`entry.js`和
`index.html`。如果有不清晰的地方，可以查看repo内具体代码。

6.  在`settings.py`里面把以下的代码变成最下的。

```python
# /madscience/settings.py

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/1.11/howto/static-files/

# STATIC_ROOT = os.path.join(PROJECT_ROOT, 'staticfiles')
# STATIC_URL = '/static/'
#
# # Extra places for collectstatic to find static files.
# STATICFILES_DIRS = [
#     os.path.join(PROJECT_ROOT, 'static'),
# ]

STATIC_ROOT = os.path.join(PROJECT_ROOT, 'frontend')
# STATIC_ROOT = os.path.join(PROJECT_ROOT, 'staticfiles')

STATIC_URL = '/static/'

# Extra places for collectstatic to find static files.
STATICFILES_DIRS = (
    os.path.join(os.path.join(BASE_DIR, 'frontend'),'static'),
)
```
这个是告诉Django后台在哪儿文件夹找那一个React的数据户。用`/frontend/webpack.config.js`跟`/madscience/settings.py`可以实现这个功能

7. 最后的需要是创造Django后台的URL：

```python
# /madscience/urls.py
from django.conf.urls import url
from django.contrib import admin

from django.views.generic import View
from django.http import HttpResponse

import os

class ReactAppView(View):
    def get(self, request):
        try:

            with open(os.path.join('frontend', 'index.html')) as file:
                return HttpResponse(file.read())

        except :
            return HttpResponse(
                """
                index.html not found ! build your React app !!
                """,
                status=501,
            )


urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^',ReactAppView.as_view())
]
```
这以上是在`urls.py`里面的代码。
