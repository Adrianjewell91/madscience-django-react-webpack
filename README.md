# 中文：A django/react skeleton using Webpack and Heroku Django template.

### 怎样把Django后台跟React.js利用webpack和起来的网路应用一小部分。

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
`index.html`。以上的代码可以表现怎么写。

6.  再`settings.py`把以下的代码变成最下的。

```python
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



1. Install npm, modules
1.5 Create Webpack config file.
2. Create frontend folder,
3. Create url and React App Routes
4. Create React entry file and information
