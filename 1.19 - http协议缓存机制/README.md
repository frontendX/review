# http协议缓存机制
### http缓存的流程
![http缓存的流程](https://upload-images.jianshu.io/upload_images/1726248-5cfbc30851793715.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### http报文的组成

**常见的请求头：**
```
Accept: text/html,image/*                                      #浏览器可以接收的类型
Accept-Charset: ISO-8859-1                                     #浏览器可以接收的编码类型
Accept-Encoding: gzip,compress                                 #浏览器可以接收压缩编码类型
Accept-Language: en-us,zh-cn                                   #浏览器可以接收的语言和国家类型
Host: www.lks.cn:80                                            #浏览器请求的主机和端口
If-Modified-Since: Tue, 11 Jul 2000 18:23:51 GMT               #某个页面缓存时间
Referer: http://www.lks.cn/index.html                          #请求来自于哪个页面
User-Agent: Mozilla/4.0 compatible; MSIE 5.5; Windows NT 5.0   #浏览器相关信息
Cookie:                                                        #浏览器暂存服务器发送的信息
Connection: close1.0/Keep-Alive1.1                             #HTTP请求的版本的特点
Date: Tue, 11 Jul 2000 18:23:51GMT                             #请求网站的时间
Allow:GET                                                      #请求的方法 GET 常见的还有POST
Keep-Alive:5                                                   #连接的时间；5
Connection:keep-alive                                          #是否是长连接
Cache-Control:max-age=300                                      #缓存的最长时间 300s
```

### http缓存规则
HTTP缓存有多种规则，根据是否需要重新向服务器发起请求来分类，可以将其分为两大类(**强制缓存，对比缓存**)，强制缓存如果生效，不需要再和服务器发生交互，而对比缓存不管是否生效，都需要与服务端发生交互。

两类缓存规则可以同时存在，强制缓存优先级高于对比缓存，也就是说，当执行强制缓存的规则时，如果缓存生效，直接使用缓存，不再执行对比缓存规则。

#### 强制缓存
对于强制缓存来说，`header` 中会有两个字段来标明失效规则（`Expires/Cache-Control`），指的是当前资源的有效期。

![强制缓存](https://upload-images.jianshu.io/upload_images/1726248-051c55557f67e5a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为方便大家理解，我们认为浏览器存在一个缓存数据库,用于存储缓存信息。<br/>
在客户端第一次请求数据时，此时缓存数据库中没有对应的缓存数据，需要请求服务器，服务器返回后，将数据存储至缓存数据库中。

**Expires**
`Expires` 的值为服务端返回的到期时间，在响应http请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据，而无需再次请求。`Expires` 的一个缺点就是，返回的到期时间是服务器端的时间，这样存在一个问题，比较的时间是客户端本地设置的时间，所以有可能会导致差错，所以在 `HTTP 1.1` 版开始，使用 `Cache-Control` 替代。

**Cache-Control**
用于定义所有的缓存机制都必须遵循的缓存指示，这些指示是一些特定的指令，包括 `public、private、no-cache` (表示可以存储，但在重新验正其有效性之前不能用于响应客户端请求)、`no-store、max-age、s-maxage以及must-revalidate` 等；`Cache-Control` 中设定的时间会覆盖 `Expires` 中指定的时间；

#### 对比缓存
对比缓存，顾名思义，需要进行比较判断是否可以使用缓存。

浏览器第一次请求数据时，服务器会将缓存标识与数据一起返回给客户端，客户端将二者备份至缓存数据库中。

再次请求数据时，客户端将备份的缓存标识发送给服务器，服务器根据缓存标识进行判断，判断成功后，返回304状态码，通知客户端比较成功，可以使用缓存数据。

![对比缓存](https://upload-images.jianshu.io/upload_images/1726248-558bcb90a1cf5fa0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在对比缓存生效时，状态码为304，并且报文大小和请求时间大大减少。<br/>
原因是，服务端在进行标识比较后，只返回header部分，通过状态码通知客户端使用缓存，不再需要将报文主体部分返回给客户端。<br/>
对于对比缓存来说，缓存标识的传递是我们着重需要理解的，它在请求header和响应header间进行传递，一共分为两种标识传递，接下来，我们分开介绍。

**Last-Modified：**
服务器在响应请求时，告诉浏览器资源的最后修改时间。

**If-Modified-Since：**
再次请求服务器时，通过此字段通知服务器上次请求时，服务器返回的资源最后修改时间。
服务器收到请求后发现有头 `If-Modified-Since` 则与被请求资源的最后修改时间进行比对。
若资源的最后修改时间大于 `If-Modified-Since`，说明资源又被改动过，则响应整片资源内容，返回状态码200；
若资源的最后修改时间小于或等于 `If-Modified-Since`，说明资源无新修改，则响应HTTP 304，告知浏览器继续使用所保存的cache。

**Etag/If-None-Match规则（优先级高于Last-Modified/If-Modified-Since）**
**Etag：**
服务器资源的唯一标识符, 浏览器可以根据 `ETag` 值缓存数据, 节省带宽. 如果资源已经改变，`ETag` 可以帮助防止同步更新资源的相互覆盖。`ETag` 优先级比 `Last-Modified` 高.

**If-None-Match：**
再次请求服务器时，通过此字段通知服务器客户段缓存数据的唯一标识。
服务器收到请求后发现有头 `If-None-Match` 则与被请求资源的唯一标识进行比对，不同，说明资源又被改动过，则响应整片资源内容，**返回状态码200**；
相同，说明资源无新修改，则**响应HTTP 304**，告知浏览器继续使用所保存的`cache`。

既然已经有了 `Last-Modified` 已经能够知道本地缓存是否是最新的了，为什么还需要 `Etag` 呢？
主要是基于以下几个原因：
- `Last-Modified` 标注的最后修改时间只能精确到秒，如果有些资源在一秒之内被多次修改的话，他就不能准确标注文件的新鲜度了；
- 如果某些资源会被定期生成，当内容没有变化，但 `Last-Modified` 却改变了，导致文件没使用缓存；
- 有可能存在服务器没有准确获取资源修改时间，或者与代理服务器时间不一致的情形。

`Etag` 是服务器自动生成或者由开发者生成的对应资源在服务器端的唯一标识符，能够更加准确的控制缓存。`Last-Modified` 与 `ETag` 是可以一起使用的，服务器会优先验证 `ETag`，一致的情况下，才会继续比对 `Last-Modified`，最后才决定**是否返回304**。在图二中，我们可以清楚的看到 `Request Headers` 里的 `If-None-Match` 和 `Response Headers Etag` 值是完全一致的，所以**返回304**。

此外，**用户操作也与缓存有关系**
![用户操作也与缓存有关系](https://upload-images.jianshu.io/upload_images/1726248-43d4aa0a4a064a2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

通过上表我们可以看到，当用户在按 `F5` 进行刷新的时候，会忽略
 `Expires/Cache-Control` 的设置，会再次发送请求去服务器请求，而 `Last-Modified/Etag` 还是有效的，服务器会根据情况判断**返回 304 还是 200**；而当用户使用 `Ctrl+F5` 进行强制刷新的时候，只是所有的缓存机制都将失效，重新从服务器拉去资源。
