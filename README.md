# My-notes
1.onSubmit="return checkForm();" 方法中：
在ajax里直接写return false，是无效的。
因为它返回的是ajax中的success方法，而不是checkForm()。
因此可以设置一个全局变量用来做判断，实现停止的效果。
注意：ajax一定要是同步的。
总结：
重要的两个点：异步请求  +  全局变量的判断
```javascript
    function checkForm(){
        var cname = $("#cname").val();
        var randomSms = $("#randomSms").val();
        n = false;//全局变量，以便下面做判断
        $.ajax({
            type: "POST",
            url: "/1/mobile/jsCode",
            data: {cname:cname,randomSms:randomSms},
            dataType: "json",
            async: false,//一定要是同步请求，否则会跳转；（ajax默认是异步的）
            success: function(obj){
                if (obj['error'] == '1') {
                    alert(obj['message']);
                } else {
                    n = true;
                    alert(obj['message']);
                }
            }
        });
        if(n) {
             return true;
        }else{
            return false;
        }
    }
```


2.在使用jq1.7版本的的情况下，用live绑定第二次触发的事件无效，解决办法是，用jq1.7以上的版本on事件代替。

3.localStorage封装
```javascript
function setStorage(key,value){
        if(!window.localStorage){
            alert("浏览器不支持localstorage");
            return false;
        }else{
            var storage=window.localStorage;
            storage.setItem(key,value);
        }
    }
    function getStorage(key){
        if(!window.localStorage){
            alert("浏览器不支持localstorage");
        }else{
            var storage=window.localStorage;
            var key=storage.getItem(key);
            return key;
        }
}
```
4.解决ios微信返回上一个页面不刷新
```javascript
(function() {
    pushHistory();
    var bool = false;
    setTimeout(function() {
        bool = true;
    }, 1500);
    window.addEventListener("popstate", function(e) {
        if (bool) {
            location.href = '#';
        }
        pushHistory();

    }, false);
});

function pushHistory() {
    var state = {
        title: "title",
        url: "#"
    };
    window.history.pushState(state, "title", "#");
}
```
4.vue.js如何在标签属性中插入变量参数  =>  v-bind:属性="'字符串'+自定义变量"。

5.display: inline-block 左右两个元素不对齐 => 父元素设置font-size: 0;里面元素字体单独设置，图标元素设置vertical-align: top.

6.
```javascript
    alert(typeof(false) === 'boolean'); //true
    alert(typeof(0) === 'number'); // true
    alert(typeof("") === 'string'); // true
    alert(typeof(null) === 'object'); // true
    alert(typeof undefined === 'undefined'); //true
    alert(false == undefined); //false
    alert(false == null); //false
    alert(false == 0); //true
    alert(false == ""); //true
    alert(null == undefined); //true
    
    // 把null和undefined归为一类，称为"空值"。
    // 0、空字符串和false归为一类，称为"假值"；
```
7.localstorage原生是不支持设置过期时间的，想要设置的话，就只能自己来封装一层逻辑来实现
```javascript
function set(key,value){
  var curtime = new Date().getTime();//获取当前时间
  localStorage.setItem(key,JSON.stringify({val:value,time:curtime}));//转换成json字符串序列
}
function get(key,exp)//exp是设置的过期时间
{
  var val = localStorage.getItem(key);//获取存储的元素
  var dataobj = JSON.parse(val);//解析出json对象
  if(new Date().getTime() - dataobj.time > exp)//如果当前时间-减去存储的元素在创建时候设置的时间 > 过期时间
  {
    console.log("expires");//提示过期
  }
  else{
    console.log("val="+dataobj.val);
  }
}
```
8.验证
```javascript
        var is = 0;
        if (adviserAddWorld == '') {
            $('.error-text3').show();
            is = 1;
        } else {
            $('.error-text3').hide();
        }
        if (adviserAddName == '') {
            $('.error-text4').show();
            is = 1;
        } else {
            $('.error-text4').hide();
        }
        if (adviserAddMail == '' || !regEmail.test(adviserAddMail)) {
            $('.error-text5').show();
            is = 1;
        } else {
            $('.error-text5').hide();
        }
        if (adviserAddPassword == '') {
            $('.error-text6').show();
            is = 1;
        } else {
            $('.error-text6').hide();
        }
        if(is === 1){
        return;
        }else{
           ////////
        }
```
9.价格实时计算
```javascript
   var ss = 0;
    var pricet = [];

    function cha() {
        var sum = 0; // 年计算
        var daySum = 0; // 天计算
        pricet.length = 0;
        ss = 0;
        $(".current-and-price").html('');
        $("input[type='checkbox']:checked").each(function() {
            var ssa = parseInt($(this).parents('.display-goods-li').find('.totalPrice').text());
            var repeatCheck = $(this).parents(".shangpin-hunhe.ertou").find(".shangpin-ming input").val();
            var repeatCheckName = $(this).parents(".shangpin-hunhe.ertou").find(".shangpin-ming input").attr("name");
            var termType = $(this).parents(".shangpin-hunhe.ertou").find(".termType").val();
            pricet.push(ssa)
            if (termType == 4) {
                daySum += parseInt(termType);
                $(".xinzeng-day").show();
                $(".addday").show();

            } else {
                sum += parseInt(repeatCheck);
                $('.addyear').show();
                $('.xinzeng-year').show();

            }

        })
        $(".xinzeng-year").text(sum);
        $(".xinzeng-day").text(daySum);
        for (var i = 0; i < pricet.length; i++) {
            ss += pricet[i];
        }
        // console.log(pricet)
        // console.log(ss)
        var span = '<span>原价：￥<span class="data-discount">' + ss + '</span></span>';
        $(".current-and-price").append(span);

    }
```
10.返回上页记住上页入口
```javascript
                <ul class="order-tab-ul">
                    <li class="skygreen indexN1" indexN='num1'>全部订单</li>
                    <li class="indexN2" id="sta-li" indexN='num2'>待支付<i class="status_i">(<span id="status_nopay">0</span>)</i></li>
                    <input type="text" id="status-hide" style="display: none;">
                    <li class="indexN3" indexN='num3'>已支付</li>
                    <li class="indexN4" indexN='num4'>支付超时</li>
                    <li class="indexN5" indexN='num5'>已撤单</li>
                </ul>
                
                 var strStoreDate = window.localStorage ? localStorage.getItem("indexN") : Cookie.read("indexN");
        if(!strStoreDate){
               $('.indexN1').trigger('click');
        }else{
        switch (strStoreDate) {
            case 'num1':
                indexNf('.indexN1');
                break;
            case 'num2':
                indexNf('.indexN2');
                break;
            case 'num3':
                indexNf('.indexN3');
                break;
            case 'num4':
                indexNf('.indexN4');
                break;
            case 'num5':
                indexNf('.indexN5');
                break;
        }

        function indexNf(obj) {
           $(obj).trigger('click');
        }
        
         $('.order-tab-ul li').click(function() {
        $(this).addClass('skygreen').siblings('li').removeClass('skygreen')
        var $index = $(this).index();
        indexN = $(this).attr('indexN');
        if (window.localStorage) {
            localStorage.setItem("indexN", indexN);
        } else {
            Cookie.write("indexN", indexN);
        }
    })
```
11.
```javascript
        var a = [];
		$('ul li').each(function(i,v){
			var productId = $(v).attr('productId');
			var totalAmount = $(v).attr('totalAmount');
			var offlineTime = $(v).attr('offlineTime');
			a.push({
				productId:productId,
				totalAmount:totalAmount,
				offlineTime:offlineTime
			})
		})
		```
12.微信内置浏览器浏览H5页面弹出的键盘遮盖文本框的解决办法
```javascript
$(function () {
window.addEventListener("resize", function () {
if (document.activeElement.tagName == "INPUT" || document.activeElement.tagName == "TEXTAREA") {
window.setTimeout(function () {
document.activeElement.scrollIntoViewIfNeeded();
}, 0);
}
})
})
```
13.中英文切换
```javascript
mounted: function() {
        var storage = window.localStorage;
        var zh = storage.getItem('zh');
        var en = storage.getItem('en');
       
        if(!zh && !en){
             storage.setItem("zh", 1);
        }
        if (zh) {
             storage.removeItem("en", 1);
        }
        if (en) {
             storage.removeItem("zh", 1);
        }
    },
     methods: {
        changeLang1: function(lg) {
            setCookie("lang", lg, 365);
            var storage = window.localStorage;
            storage.setItem("zh", 1);
            storage.removeItem("en");
            location.reload();
        },
        changeLang2: function(lg) {
            setCookie("lang", lg, 365);
            var storage = window.localStorage;
            storage.setItem("en", 1);
            storage.removeItem("zh");
            location.reload()
        }
     }
```
14.h5上传文件并打开
```javascript
  <input type="file" id="input">
  <span id="preview"></span>

(function(){
  var input = document.querySelector('#input');
  var span = document.querySelector('#preview');
  input.addEventListener('change', function(e){
    handFile(e.target.files[0]);
  });
 
  function handFile(file){
    console.log('hand');
    var reader = new FileReader();
    reader.onload = function(e){
      span.innerText = e.target.result;
    };
    reader.readAsText(file);
  }
})();
```
15.json转化
```javascript
var arr = [{
    "aaa": {
        "name": "小敏",
        "price": "18",
        "count": 2,
        "num": 5
    },
    "bbb": {
        "name": "小李",
        "price": "18",
        "count": 1,
        "num": 11
    },
    "ccc": {
        "name": "小华",
        "price": "18",
        "count": 1,
        "num": 18
    }
}]
var arr1 = [
      {"id":"aaa","count":2},
      {"id":"bbb","count":1},
      {"id":"ccc","count":1}
]
var newArr = [];
for(var tmp in arr){
    for(var key in arr[tmp]){
        console.log(key);
        newArr.push({"id":key,"count":arr[tmp][key].count});
    }
}
```
16.localStorage设置过期时间
```javascript
function set(key,value){
  var curtime = new Date().getTime();//获取当前时间
  localStorage.setItem(key,JSON.stringify({val:value,time:curtime}));//转换成json字符串序列
}
function get(key,exp)//exp是设置的过期时间
{
  var val = localStorage.getItem(key);//获取存储的元素
  var dataobj = JSON.parse(val);//解析出json对象
  if(new Date().getTime() - dataobj.time > exp)//如果当前时间-减去存储的元素在创建时候设置的时间 > 过期时间
  {
    console.log("expires");//提示过期
  }
  else{
    console.log("val="+dataobj.val);
  }
}
```
17.字母中文a-z 排序
```javascript
var arr = [9,8,7,6,5,1,'在', '我', '里', '阿','z','a','h','m'];
arr.sort(function(a,b){return a.toString().localeCompare(b)}) 
```
18.js中使用new Date(str)创建时间对象不兼容firefox和ie的解决方式
```javascript
var date = new Date(str.replace("-", "/").replace("-", "/"));
```

19.     给两个数组相同元素转对象加个标识（小程序）
```javascript
        // var p2 = "hello world good for you that's impossible heyhey";
        // var meText = "hello good day for your ok you are right";
   
        var p2 = app.globalData.testResultInfo.bd_sentence;
        var meText = app.globalData.testResultInfo.sentence;
        console.log("from bd:"+p2);
        console.log("standard:" + meText);
        var sameIndex = util.LCS(p2, meText); //0 2 3 4
        function trim(str) {
            return str.replace(/(^\s*)|(\s*$)/g, "");
        }
        var sameIndexArr = trim(sameIndex).split(' '); // ["0", "2", "3", "4"]
        var meTextArr = meText.split(' '); //标准字符串
        var meTextObj = []; //返回一个对象
        for(let i  = 0; i < meTextArr.length; i++)
        {
            let temp = {'text':meTextArr[ i ],'status':true};
            meTextObj.push(temp);
        }
        for(let j = 0 ; j < sameIndexArr.length; j++)
        {
           if(sameIndexArr[ j ] != "")
           {
             meTextObj[sameIndexArr[j]].status = false;
           }
        }
	console.log(meTextObj)
```

20. 微信小程序canvas画图，用户userInfo图像显示不了，原因是开发者工具没有去掉"校验..."前面的勾选，还有小程序开发设置后台，没有设置downloadFile合法域名，这里是设置用户图像的前面的域名"https://wx.qlogo.cn"
21. 添加数据concat
```javascript
//假设这一段是我们要新增的数组
var newarray = [{
        name:'增加的数据--'+new Date().getTime() ,
}];
//向前--用newarray与this.data.list合拼
this.data.list = newarray.concat(this.data.list);
//向后--用this.data.list与newarray合拼
this.data.list = this.data.list.concat(newarray);
```
22. 删除数据splice()删除数据，然后返回被删除的数据
```javascript
//删除
  remove:function (e){
    var dataset = e.target.dataset;
    var Index = dataset.index;
    //通过index识别要删除第几条数据，第二个数据为要删除的项目数量，通常为1
    this.data.list.splice(Index,1);
    //渲染数据
    this.setData({
        list:this.data.list
    });
  }
```

23.当前时间跟本周最后一天时间比较
```javascript
// 当周最后周日时间
        function weekLastDay() {
            var Nowdate = new Date();
            var WeekFirstDay = new Date(Nowdate - (Nowdate.getDay() - 1) * 86400000);
            var WeekLastDay = new Date((WeekFirstDay / 1000 + 6 * 86400) * 1000);
            var f = new Date((WeekFirstDay / 1000 + 6 * 86400) * 1000);
            var m = Number(WeekLastDay.getMonth()) + 1
            return WeekLastDay.getFullYear() + "-" + m + "-" + WeekLastDay.getDate();
        }
        // 当前时间
        function WeekNowDay() {
            var Nowdate = new Date();
            var n = Nowdate.getFullYear();
            var y = Nowdate.getMonth() + 1;
            var r = Nowdate.getDate();
            return n + '-' + y + '-' + r
        }
        var weekLastDay1 = weekLastDay(); //2017-12-24
        var WeekNowDay1 = WeekNowDay(); //2017-12-21
```
24，
微信小程序：防止多次点击跳转（函数节流）
```javascript
function throttle(fn, gapTime) {
    if (gapTime == null || gapTime == undefined) {
        gapTime = 1500
    }

    let _lastTime = null

    // 返回新的函数
    return function () {
        let _nowTime = + new Date()
        if (_nowTime - _lastTime > gapTime || !_lastTime) {
            fn.apply(this, arguments)   //将this和参数传给原函数
            _lastTime = _nowTime
        }
    }
}
module.exports = {
  throttle: throttle
}

const util = require('../../utils/util.js')

Page({
    data: {
        text: 'tomfriwel'
    },
    onLoad: function (options) {

    },
    tap: util.throttle(function (e) {
        console.log(this)
        console.log(e)
        console.log((new Date()).getSeconds())
    }, 1000)
})

```
