---
layout: post
status: publish
title: Prototype Validation using Ajax

date: '2007-06-05 22:34:00 +0100'
date_gmt: '2007-06-05 22:34:00 +0100'
tags:
- javascript
- code
---
I've been using the excellent Prototype validation over at <a href="http://tetlaw.id.au/view/javascript/really-easy-field-validation">dexagogo</a> but recently wanted to have a a feature so as when you type a username in a sign up form it fires a real time lookup to check the username availability. After a bit of Googling and some experimentation I came up with the following. Hopefully it'll save you some time. In this case the lookup is done via Coldfusion which encodes and returns JSON.
The form bit is pretty standard.
```

<div class="form-row">
<div class="field-label"><label for="field1">Username</label>:</div>
<div class="field-widget"><input name="field1" id="field1" class="text required validate-username" title="Enter yourusername." /></div>
</div>
```
The Javascript bit included on this page in an external JS file.
```
Validation.addAllThese([
    ['validate-username','Sorry, that username is already taken.',function(v,elm) {
        if(elm._ajaxValidating && elm._hasAjaxValidateResult) {
            elm._ajaxValidating = false;
            elm._hasAjaxValidateResult = false;
            return elm._ajaxValidateResult;
        }
        var sendRequest = function() {
            new Ajax.Request('/path/to/check/file?username='+v+'&r='+Math.random(),{
                onSuccess : function(response) {
                    elm._ajaxValidateResult = eval(response.responseText);
                    elm._hasAjaxValidateResult = true;
                    Validation.test('validate-username',elm);
                }
            });
            elm._ajaxValidating = true;
            return true;
        }
        return elm._ajaxValidating || Validation.get('IsEmpty').test(v) || sendRequest();
     }]
]);
```
The Javascript bit in the page.
```
<script language="javascript" type="text/javascript">
	var valid_register = new Validation('register', {immediate:true,stopOnFirst:true,onSubmit:false});
</script>
```
The form is submitted with an Event.observe on the submit button I have as well. 
And so concludes my first geekish post for a while, it feels strangely good!
