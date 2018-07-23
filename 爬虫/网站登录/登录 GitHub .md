```python
import requests
from lxml import etree

s = requests.session()

# 首先访问首页 取得 token
html = s.get('https://github.com/login').text
html = etree.HTML(html)
token = html.xpath('//input[@name="authenticity_token"]/@value')[0]

data = {
    'commit': 'Sign in',
    'utf8': '✓',
    'authenticity_token': token,
    'login': '1120159006@qq.com',
    'password': 'leixianyang1996'
}

# 然后进行登录
re = s.post(data=data, url='https://github.com/session')

# 然后就可以进入登录后的页面
index = s.get('https://github.com/settings/profile')
print(index.text)

```

