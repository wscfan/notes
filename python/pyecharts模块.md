# pyecharts模块

## 1、绘制柱状图

```python
from pyecharts.faker import Faker
from pyecharts import options as opts
from pyecharts.charts import Bar

bar = Bar()
bar.set_global_opts(title_opts=opts.TitleOpts(title='Bar实例', subtitle='我是副标题'))
bar.add_xaxis(Faker.choose())
bar.add_yaxis('商家', Faker.values())
bar.render()
```

