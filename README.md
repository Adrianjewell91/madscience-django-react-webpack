# 中文：A django/react skeleton using Webpack and Heroku Django template.

### 如何利用webpack把Django后端与React.js前段连接起来。
### Thank you to Chris Guan for proof-reading. 谢谢Chris Guan修改这个文章。

## How-to （操作步骤)

1. 把Django Heroku Template下载， 阅读not_README.md内的教程。可能需要用到： `python manage.py runserver`.

2. 没有提示错误，用`npm`结合数据库的方法： `npm init --yes`

3. 陈宫后，运行： `npm install --save webpack
react
react-dom
react-router-dom
redux
react-redux
babel-core
babel-loader
babel-preset-react
babel-preset-es2015`

4. 与数据库连接成功后, 修改webpack.config.js：

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
那一些复数看似复杂， 目的是让Django后端连接上React.js前端的资源。

5. 创建一个叫`frontend`文件夹, 在里面创建一个文件夹叫 `static`, 然后再分别创建`entry.js`和`index.html`两个文件。如果有不清晰的地方，可以查看repo内具体代码。

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
这个是告诉Django后台在哪个文件夹能找那一个React的数据库。`/frontend/webpack.config.js`跟`/madscience/settings.py`可以实现这个功能。

7. 最后，需要创建Django后台的URL：

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
以上是在`urls.py`里面的代码。
