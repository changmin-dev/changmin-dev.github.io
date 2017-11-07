---
layout: post
title: scrapy의 SgmlLinkExtractor문제(python3)
---

`파이썬으로 웹크롤러 만들기`책의 3장에 scrapy를 사용하는 부분이 있는데 잘 되지 않았다. 나는 python3를 사용하였고 책에서는 scrapy는 python2를 사용하였다. 찾아 보니 python3에서는 `SgmlLinkExtractor`을 지원하지 않는다고 한다. 또한 python2에서도 deprecated되었다고 한다. `SgmlLinkExtractor`을 사용하니 문제 없이 작동했다.    

다음과 같이 import하고 사용하면 된다.
```python
from scrapy.linkextractors import LinkExtractor
```
