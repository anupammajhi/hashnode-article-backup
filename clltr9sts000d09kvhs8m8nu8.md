---
title: "JS: Decimal and Hexadecimal"
datePublished: Fri Jul 22 2016 16:47:33 GMT+0000 (Coordinated Universal Time)
cuid: clltr9sts000d09kvhs8m8nu8
slug: js-decimal-and-hexadecimal
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693088539139/8c5e18fc-d9de-406d-9564-e5896f587bb6.png
tags: js, javascript, conversion, hexadecimal, decimal

---

This is pretty interesting how you can convert not just standard decimal or hexadecimal numbers but any number with a base between 2 and 32. Here’s how:

```javascript
var myDecimalNum = 10; 
myDecimalNum.toString(16); 

// result : a
```

```javascript
var myHexNum = "a"; 
parseInt(myHexNum,16); 

// result : 10
```

And not just base 16 (hex), you can do it for any base. Try different bases and have fun.