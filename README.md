# My-notes
1.onSubmit="return checkForm();" 方法中：
在ajax里直接写return false，是无效的。
因为它返回的是ajax中的success方法，而不是checkForm()。
因此可以设置一个全局变量用来做判断，实现停止的效果。
注意：ajax一定要是同步的。
<pre>
<code>
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
    </code>
</pre>
总结：
重要的两个点：异步请求  +  全局变量的判断

2.在使用jq1.7版本的的情况下，用live绑定第二次触发的事件无效，解决办法是，用jq1.7以上的版本on事件代替。
3.localStorage封装
<pre>
<code>
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
</code>
</pre>
4.解决ios微信返回上一个页面不刷新
<pre>
<code>
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
</code>
</pre>
4.vue.js如何在标签属性中插入变量参数  =>  v-bind:属性=“ ‘字符串’+自定义变量名”
5.display: inline-block 左右两个元素不对齐 => 父元素设置font-size: 0;里面元素字体单独设置，图标元素设置vertical-align: top; 

