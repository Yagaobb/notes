Javascript使用postMessage对iframe跨域通信
页面a 用iframe 嵌入 页面b。


```
A.html
<input id="sendText" type="text">
<div id="sendBtn" onclick="test()">发送</div>
<iframe id="iframe" src="B.html"></iframe>
<script>
  var sendBtn = document.getElementById('sendBtn');
  var iframe = document.getElementById('iframe');
  // sendBtn.addEventListener('click', function (e) {})// 监听点击事件 addEventListener前面用getElementById获取id，$("#sendBtn")和document.getElementById('sendBtn')获取id，不一样
  // $("#iframe").on('load', function () { // 一进入页面进行加载
  function test(){//通过点击事件
    var sendText = $('#sendText').val();
    //要发送的数据
    var data = {
      act: 'article',
      msg: {
        subject: sendText,
        author: 'yagao'
      }
    }
    //发送消息(向谁发送消息)
    iframe.contentWindow.postMessage(data, '*');//$("#iframe")[0].contentWindow.postMessage(data, '*');
  };

  // 注册消息事件监听，对来自iframe框架的消息进行处理
  window.addEventListener('message', function (e) {
    if (e.data.act == 'response') {
      console.log(e.data.msg.answer);
    } else {
      console.log('未定义的消息：' + e.data.act);
    }
  }, false);
</script>
```
```
B.html
<div>接收消息：</div>
<div id="receive"></div>
<script>
  // 注册消息事件监听，对来自父级页面的消息进行处理
  window.addEventListener('message', function (e) {
    if (e.data.act == 'article') {
      console.log(e.data.msg.subject);
      $("#receive").text(e.data.msg.subject)
      //向父窗框返回响应结果
      window.parent.postMessage({
        act: 'response',
        msg: {
          answer: '我接收到拉'
        }
      }, '*');
    } else {
      console.log('未定义的消息：' + e.data.act);
    }
  });
</script>
```

个人理解：
主要通过postMessage方法发送消息，通过addEventListener去监听message，进行接收消息
postMessage() 方法用于安全地实现跨源通信。
注意点：
父页面a 传数据时一定要在 子页面 addEventlistener 之后，否则子页面是接收不到数据的。所以我们在使用时，可以参考通信中的三次握手。 子页面addEventlistener 之后 ，采用 postmessage send 信息给父页面，告诉父页面，子页面已经准备好了，可以传数据了。如此避免数据丢失！！！

