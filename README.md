# 记性越来越差了，记录一些实用的js工具函数吧，不定期更新
### 1. 生成随机ID
  ```Math.random().toString(36).substring(2)```
### 2. 获取URL查询参数
  ```
  const p = {}
  search = '?foo=a&bar=b&baz='
  search.replace(/([^?&=]*)=([^&]*)/g, (_, key, val) => p[key] = val)
  console.log(p) // {foo: 'a', bar: 'b', baz: ''}
  ```
  ### 3. 数组混淆
  ```arr.sort(() => Math.random() - 0.5)```
