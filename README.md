# My-notes
1.onSubmit="return checkForm();" 方法中：
在ajax里直接写return false，是无效的。
因为它返回的是ajax中的success方法，而不是checkForm()。
因此可以设置一个全局变量用来做判断，实现停止的效果。
注意：ajax一定要是同步的。
<code>
<form action="/1/mobile/codeJsLogin" onSubmit="return checkForm();" id="login_form" method="post"></form>
</code>
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
总结：
重要的两个点：异步请求  +  全局变量的判断
