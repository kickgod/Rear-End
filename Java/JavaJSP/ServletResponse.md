### [Response 对象](#top) <b id="top"></b> :maple_leaf:

----
`Request 对象是一个专门用来存储HTTP 请求信息的对象`

- [x] :maple_leaf: [`Response 对象的由来`](#response) 
- [x] :maple_leaf: [`Response 功能`](#func) 



##### [Response 对象的由来](#top)  <b id="response"></b>
`封装HTTP响应信息的对象 处理渲染细节 负责将响应信息发送给浏览器`

##### [Response 功能](#top)  <b id="response"></b>
* `设置响应编码格式 设置响应头信息`
  * `addHeader(java.lang.String name, java.lang.String value)` ：`添加具有给定名称和值的响应标头。`
  * `setHeader(java.lang.String name, java.lang.String value)` :`设置具有给定名称和值的响应标头。`
  * `resp.setContentType("text/html;charset=UTF-8");`:`设置响应格式为 UTF8`
