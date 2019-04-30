# 好记性不如烂笔头，记录下比较实用js工具函数

 ### 生成随机ID
  ```Math.random().toString(36).substring(2)```
 ### 获取URL查询参数
  ```
  const p = {}
  search = '?foo=a&bar=b&baz='
  search.replace(/([^?&=]*)=([^&]*)/g, (_, key, val) => p[key] = val)
  console.log(p) // {foo: 'a', bar: 'b', baz: ''}
  ```
 ### 数组混淆
  ```arr.sort(() => Math.random() - 0.5)```
