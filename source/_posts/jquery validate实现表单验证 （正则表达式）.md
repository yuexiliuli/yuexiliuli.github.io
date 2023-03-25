---
title: jquery validate实现表单验证 （正则表达式）
urlname: upvo9w
date: '2021-05-13 16:29:30 +0800'
tags: [JavaScript]
categories: [JavaScript]
---

# 一、目的

```cpp
为了更好地实现人机交互，使用jQuery封装库中的validate插件，在用户填写表单时，可以快速地对用户填写的数据进行验证，并做出反馈。
```

# 二、validate 插件简介

```
validate()是插件的核心方法，定义了基本的校验规则和一些有用的配置项。
```

rule:设置表单的验证规则；
messages:设置表单不符合验证规则的提示信息；
debug:如果这个参数为 true,那么表单还会提交，只进行检查，调试时十分方便。

required:必填
minlength:最小长度
maxlength:最大长度
rangelength:长度范围，以数组呈现[2，10]，表示表单输入长度为 2 到 10 位
remote:可以通过发现 GET 或者 POST 请求进行远程验证，与 Ajax 的验证进行比较。就是通过

```javascript
ajax实现的
｛
    url:
    type:默认为GET请求
    data:发送的数据
｝
发送GET请求例子：
check:{
       required:true,
       remote:{
       url:"__CONTROLLER__/check?check="+$("#icode").val
       //__CONTROLLER__表示当前控制器
       }
}
```

## 基本的校验规则

序号 规则 描述
1 required:true 必须输入的字段。
2 remote:"check.php" 使用 ajax 方法调用 check.php 验证输入值。
3 email:true 必须输入正确格式的电子邮件。
4 url:true 必须输入正确格式的网址。
5 date:true 必须输入正确格式的日期。日期校验 ie6 出错，慎用。
6 dateISO:true 必须输入正确格式的日期（ISO），例如：2009-06-23，1998/01/22。只验证格式，不验证有效性。
7 number:true 必须输入合法的数字（负数，小数）。
8 digits:true 必须输入整数。
9 creditcard: 必须输入合法的信用卡号。
10 equalTo:"#field" 输入值必须和 #field 相同。
11 accept: 输入拥有合法后缀名的字符串（上传文件的后缀）。
12 maxlength:5 输入长度最多是 5 的字符串（汉字算一个字符）。
13 minlength:10 输入长度最小是 10 的字符串（汉字算一个字符）。
14 rangelength:[5,10] 输入长度必须介于 5 和 10 之间的字符串（汉字算一个字符）。
15 range:[5,10] 输入值必须介于 5 和 10 之间。
16 max:5 输入值不能大于 5。
17 min:10 输入值不能小于 10。

## validator 对象

validator.form()验证表单是否有效，返回 true 或者 false;
validator.element(element)验证表单中某个元素是否有效，返回 true 或者 false;
validator.resetForm()把表单恢复到验证前原来的状态；
validator.showErrors(error)针对元素显示特定的错误信息；
validator.numberOfInvalids()返回无效的元素数量；

## validator 对象的静态方法

jQuery.validator.addMethod()增加自定义的验证方法;  （即$.validator.addMethod()）
jQuery.validator.format()格式化字符串，用参数代替模板中的｛n｝;
jQuery.validator.setDefaults()修改插件默认设计；
jQuery.validator.addClassRules()为某些包含名为 name 的 class 增加组合验证类型。

```javascript
$.validator.addClassRules({
  txt: {
    required: true,
    rangelength: [2, 10],
  },
}); //这时class="txt"的类都具备了这个两个验证规则
```

获取表单元素的验证规则：    $("#username").rules();
为表单元素添加验证规则：    $("#username").rules('add',rules);
为表单元素删除验证规则：    $("#username").rules('remove',rules);

# 三、正则表达式

常用正则表达式：
用户名的正则表达式验证：`/^[\w\u4e00-\u9fa5]{2,10}/g`(含汉字)
用户名验证：`/^\w{2,10}$/`(不含汉字，只允许英文字母、数字和下画线，长度为 2-10 位)
QQ 号验证:`/^[1,9][0,9]{4,19}$/`（第一位数字不为 0，5-19 位数字）
邮箱验证：/[[1]](#fn1)+@([a-z0-9]+.)+[a-z]{2,4}/(只允许 6-16 位英文字母、数字和下画线)
手机号验证：/^1[3,5,7,8]\d{9}/i

## 代码：

```javascript
$(document).ready(function () {
  $("#table").validate({
    rules: {
      admin_name: {
        required: true,
        checkName: true,
      },
      name: {
        required: true,
      },
      admin_pwd: {
        required: true,
        checkPwd: true,
      },
      con_pwd: {
        required: true,
        equalTo: admin_pwd,
      },
      email: {
        required: true,
        checkEmail: true,
      },
      qq: {
        required: true,
        checkQQ: true,
      },
      s_page: {
        url: true,
      },
      check: {
        //required:true,
        //remote:{
        //url:"__CONTROLLER__/check?check="+$("#icode").val,
        //__CONTROLLER__表示当前控制器
        //dataType:"json",
        //}
      },
    },
    messages: {
      admin_name: {
        required: "*必填！",
        rangelength: "*长度为2到10位！",
      },
      name: {
        required: "*必填！",
      },
      admin_pwd: {
        required: "*必填！",
        rangelength: "*长度为6到16位！",
      },
      con_pwd: {
        required: "*必填！",
        equalTo: "*两次输入的密码不一致！",
      },
      email: {
        required: "*必填！",
        email: "*请输入正确的邮箱！",
      },
      qq: {
        required: "*必填！",
      },
      s_page: {
        url: "*请输入正确的网页地址！",
      },
      check: {
        required: "*必填！",
        remote: "*验证码有误！",
      },
    },
    //是否在获取焦点时验证
    //onfocusout:false,
    //是否在敲击键盘时验证
    //onkeyup:false,
    //提交表单后，（第一个）未通过验证的表单获得焦点
    focusInvalid: true,
    //当未通过验证的元素获得焦点时，移除错误提示
    focusCleanup: true,
  });

  //自定义正则表达示验证方法
  $.validator.addMethod(
    "checkQQ",
    function (value, element, params) {
      var checkQQ = /^[1-9][0-9]{4,19}$/;
      return this.optional(element) || checkQQ.test(value);
    },
    "*请输入正确的QQ号码！"
  );

  $.validator.addMethod(
    "checkEmail",
    function (value, element, params) {
      var checkEmail = /^[a-z0-9]+@([a-z0-9]+\.)+[a-z]{2,4}$/i;
      return this.optional(element) || checkEmail.test(value);
    },
    "*请输入正确的邮箱！"
  );

  $.validator.addMethod(
    "checkName",
    function (value, element, params) {
      var checkName = /^\w{2,10}$/g;
      return this.optional(element) || checkName.test(value);
    },
    "*只允许2-10位英文字母、数字或者下画线！"
  );

  $.validator.addMethod(
    "checkPwd",
    function (value, element, params) {
      var checkPwd = /^\w{6,16}$/g;
      return this.optional(element) || checkPwd.test(value);
    },
    "*只允许6-16位英文字母、数字或者下画线！"
  );
});
```

## 原文

[https://blog.csdn.net/u014800380/article/details/52106923?depth_1-](https://blog.csdn.net/u014800380/article/details/52106923?depth_1-)
