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
  ### 15. 使用 ^ 异或操作符取代三元操作符
  ```
  var foo = 0;
  var toggleFoo = function() {
    - foo = foo === 0 ? 1 : 0
    + foo = foo ^ 1
  }
  ```
  ### 16. 类型判断
  ```
  const isType = type => obj => Object.prototype.toString.call(obj) === `[object ${type}]`
  const isString = isType('String')
  const isObject = isType('Object')
  
  isString('hello world') // true
  isObject({}) // true
  ```
  ### 17. 使用void 屏蔽错误，避免后续代码中断执行
  ```
  void new Error('fail')
  ```
  ### 18. 浏览器默认访问端口号（尝试给域名添加端口号访问）
  ```
  http  : 80
  https : 443
  ```
  ### 19. 使用ls-remote查看git远程仓库上的信息
  ```
  git ls-remote --tags git@github.com:shitouplus/util.git // 查看远程仓库上的标签，如果当前目录下有.git文件夹仓库地址可省略
  ```
  ### 20. 只克隆特定的tag，有效减小服务器仓库的大小
  ```
  git clone -b 'v2.0' --single-branch --depth 1 https://github.com/git/git.git
  ```
  ### 21. 自动打包命令配置
  ```
    // 以pre和post开头的命令会在跑对应的命令时自动运行，pre是在之前运行，post是在之后运行
    "build": "node scripts/build.js",
    "prebuild": "rm -rf video_cms-0.1.0.tgz", // 删除之前生成的包
    "postbuild": "cp package.json ./build && npm pack ./build", // 复制package.json文件并打包./build目录
  ```
  ### 22. 不需要读取文件即可获取img的像素宽高，video/canvas也有对应的api
  ```
  console.log(img.naturalHeight, img.img.naturalWidth)
  ```
  ### 23. object-fit/object-position/vertical-align 修改video/img/iframe等可替换元素显示样式
 > mdn https://developer.mozilla.org/zh-CN/docs/Web/CSS/Replaced_element
  ### 24. 让页面休眠1秒
  ```
  const start = Date.now()
  while(Date.now() - start < 1000) {}
  console.log('1秒后执行')	
  ```
  ### 25. 快速删除object中value为undefined的属性, JSON.stringify第二个参数为函数可以进行遍历
  ```
  JSON.stringify(object)
  ```
  ### 25. 获取数组中最小值的索引
  ```
  arr.indexOf(Math.min(...arr))
  // 代码虽然简练但是进行了两轮比较，性能较差
  ```
  ### 26. JS原生支持base64编码/解码，浏览器支持度很高
  ```
  var encodedData = window.btoa("Hello, world"); // 编码
  var decodedData = window.atob(encodedData); // 解码
  ```
  ### 27. JS判断浏览器是否支持某css属性
  ```
  if ('fontDisplay' in document.body.style) {
     console.log('该浏览器支持font-display')
  }
  ```
  ### 28. 简易模板替换
  ```
  String.prototype.temp = function(obj) {
    return this.replace(/\$\w+$/gi, function(match) {
      return obj[match.replace(/\$/g, '')] || ''
    })
  }
  ```
  ### 29. 将秒格式化为时间
  ```
  function formatTime (second) { // second = 61
    let time = Number(second)

    let h = parseInt(time / 3600)

    let m = parseInt((time % 3600) / 60)

    let s = parseInt(time % 60)
    h = h < 10 ? '0' + h : h
    m = m < 10 ? '0' + m : m
    s = s < 10 ? '0' + s : s
    
    return h + ':' + m + ':' + s // return '00:01:01'
  }
  ```
  ### 30. 间隔不连续的for循环
  ```
  // 创建红色的画布
  var $canvas = document.createElement('canvas')
  var ctx = $canvas.getContext('2d')
  var imageData = ctx.createImageData(100, 100)

  for (var i = 0, len = imageData.data.length; i < len; i +=4) { // 每次加4，可以分别操作每个具体的值
    imageData.data[i] = 255
    imageData.data[i + 1] = 0
    imageData.data[i + 2] = 0
    imageData.data[i + 3] = 255
  }

  ctx.putImageData(imageData, 0, 0, 0, 0, 50, 50)
  document.body.appendChild($canvas)
  ```
  ### 31. 使用window.requestAnimationFrame取代setTimeout和setInterval，绘制dom
  ```
  // 使用canvas展示视频
  var $video = document.getElementById('video')
  var $canvas = document.getElementById('canvas')
  var ctx = $canvas.getContext('2d')

  $video.onloadedmetadata = function () {
    $canvas.width = $video.videoWidth
    $canvas.height = $video.videoHeight
  }

  $video.onplay = function play(timestamp) {
    if ($video.paused || $video.ended) { return }
    ctx.drawImage($video, 0, 0)

    console.log(timestamp)
    window.requestAnimationFrame(play)
  }
  ```
  ### 32. 使用 blob 计算字符串的字节数
  ```
  var str1 = 'a'
  var str2 = '我'
  var blob1 = new Blob([str1])
  var blob2 = new Blob([str2])
  
  console.log(blob1.size) // 1
  console.log(blob2.size) // 3
  ```
  ### 33. 创建特定大小的数组
  ```
  [...Array(n).keys()] // [0, 1, 2, ...., n]
  ```
  ### 34. split分割使结果中包含分隔块
  ```
  var myString = "Hello 1 word. Sentence number 2.";
  var splits = myString.split(/(\d)/);

  console.log(splits); // [ "Hello ", "1", " word. Sentence number ", "2", "." ]
  ```
  ### 35. process.env.npm_config_xxx 获取配置信息
  ```
  npm run dev --foo=bar // process.env.npm_config_foo === bar
  ```
  ### 36. 使用css属性point-event: none穿透事件
  > 注意: 当设置该属性时，无法使用chrome devtool查找该元素
  ```
  // html
  <div class="outer">
    <a href="javascript:void(0)" onclick="alert('outer')">outer</a>
    <div class="inner"></div>
  </div>
  
  // css
  <style>
	.outer {
	    position: relative;
	    width: 200px;
	    height: 200px;
	    background: #4e5d6b;
	}

	.inner {
	    position: absolute;
	    top: 0;
	    left: 0;
	    width: 100%;
	    height: 100%;
	    background: cadetblue;
	    opacity: 0.5;
	    pointer-events: none; // 当设置该属性，所有的事件都会向下层渗透，因此<a/>标签可以响应点击事件
	}
  </style>
  ```
