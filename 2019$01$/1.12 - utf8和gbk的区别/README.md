# utf8和gbk的区别

`gbk` 是在国家标准 `GB2312` 基础上扩容后兼容 `GB2312` 的标准（好像还不是国家标准）。**`gbk` 编码专门用来解决中文编码的，是双字节的，不论中英文都是双字节的。**

**`utf-8` 编码是用以解决国际上字符的一种多字节编码**，它对英文使用8位（即一个字节），中文使用24位（三个字节）来编码。对于英文字符较多的论坛则用 `utf-8` 节省空间。另外，如果是外国人访问你的 `gbk` 网页，需要下载中文语言包支持。访问 `utf-8` 编码的网页则不出现这问题。可以直接访问。

**`gbk` 包含全部中文字符；`utf-8` 则包含全世界所有国家需要用到的字符。**

经常有人问网页编写 `utf-8` 和 `GBK` 哪个编码好，根据个人需要，如果你主要做中文程序的开发，客户也主要是中国人的话就用 `gbk` 吧，因为 `utf-8` 编码的中文使用了三个字节，用 `gbk` 节省了空间。

如果做英文网站开发，还是用 `utf-8` 吧，因为 `utf-8` 中英文只占一个字节。`gbk` 中英文也是两个字节的，并且国外客户访问 `gbk` 要下载语言包。

如果你的网站是中文的，但国外用户也不少，最好也用 `utf-8` 的吧。

`utf-8` 编码的文字可以在各国各种支持 `utf-8` 字符集的浏览器上显示。
比如，如果是 `utf-8` 编码，则在外国人的英文IE上也能显示中文，而无需他们下载IE的中文语言支持包。 所以，对于英文比较多的论坛 ，使用 `gbk` 则每个字符占用2个字节，而使用 `utf-8` 英文却只占一个字节。
`utf-8` 是国际编码，它的通用性比较好，外国人也可以浏览论坛，`gbk` 是国家编码，通用性比 `utf-8` 差，不过 `utf-8` 占用的数据库比 `gbk` 大。
