## flex

[flex-shrink flex-grow flex-basic ](https://www.zhihu.com/search?type=content&q=flex-shrink flex-grow flex-basic)

### flex-basic

设置flex布局中的每个项目的宽度/高度(由flex-direction决定)

* auto

  flex项目未设置width, 则自动设置项目，宽/高度为max-content

* max-content

  最大内容宽度

* min-content

  最小内容宽度

* width

  明确指定项目宽度，优先级大于 `width`

* 0

  flex项目会被设置为相同的宽度

### flex-grow

当flex容器宽度大于flex项目宽度，剩余空间的分配规则，总量始终为1，例如：1 2 3 4 ，则四个flex项目会按1:2:3:4分配剩余空间

### flex-shrink

当flex容器小于flex项目宽度，剩余空间的分配规则

例如： flex容器宽度400px, 总项目宽度500px, 超出100px, flex-shrink: 1 2 3 4: 则超出的100px拆分为 1+2+3+4=10份，则四个项目收缩宽度为：（100/ 10）* 1 = 10px,  (100 / 10) * 2 = 20px, (100 / 10) * 3 = 30px, (100/ 10) * 4 = 40px

设置为0则不收缩

