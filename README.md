## 记录一些实用的js工具函数，不定期更新
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
  ### 4. 数组去重
  ```[...new Set(arr)]```
  ### 5. 创建特定大小的数组
  ```[...Array(3).keys()]``` // [0, 1, 2]
  ### 6. split语法，分隔符可以为正则或多个字母
  ```str.split([separator[, limit]])```
  ### 7. 给dom添加hidden属性隐藏节点
  ```<div hidden>隐藏节点</div>``` // 相当于display: none
  ### 8. 遍历对象属性，使用continue跳过继承的属性
  ``` 
  for (var key in obj) {
    if (!obj.hasOwnProperty(key)) {
      continue;
    }
    console.log(key)
  }
  ```
  ### 9. 在绑定的事件函数里使用闭包保存变量
  ```
  btn.addEventListener('click', (function() {
    var isShow = true;
    return function() {
      isShow = !isShow;
      console.log(isShow)
    }
  })())
  ```
  ### 10. 请求较慢时进行提示，较快时则不提示
  ```
  const delaydeMsg = setTimeout(() => console.log('waiting for load'), 1000)
  await sendRequest()
  clearTimeout(delayedMsg)
  ```
  ### 11. 请求较快避免进度条闪烁，异步请求增加最小时长500ms限制
  ```
  const $http = (url, params) => new Promise(res) {
	  showLoading();
	  const time = new Promise(res => {
    	setTimeout(() => res(), 500)
  	});
	  const $ajax = fetch(url, params)
	  Promise.all([time, $ajax]).then([_, data] => {
		  hideLoading();
		  res(data)
	  })
  }
  ```
  ### 12. ES6模块通过工厂函数传递数据
  ```
  // factory.js(这里假设该模块无法获取$axios)
  export default $axios => ({
    fetchList (params) {
    	return $axios.get(url, params)
    }
  })
  // api.js
  import factory from 'factory.js'
  const api = factory($axios)
  ```
  ### 13. 使用break退出循环
  ```
  function getFoo(arr) {
    var msg = 'hello '
    for(var i = 0; i < arr.length; i++) {
	if (arr[i] === 'world') {
	  msg += arr[i];
	  break;
	}
     }
     console.log(msg)
  }
  ```
  ### 14. 使用Vue.$nextTick避免合并更新
  ```
  this.step = 0;
  - this.step = 1; // print 1
  + this.$nextTick(function() {
      this.step = 1; // print 0 1
  })
  watch: {
    step(val) {
      console.log(val)
    }
  }
  ```
