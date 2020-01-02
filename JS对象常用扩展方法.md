https://blog.csdn.net/spw55381155/article/details/80193896

/*

\* 以某字符串开头判断

*注意:参数str中如果包含正则通配字符,如"."请改参数为"\\."

*/

String.prototype.startWith = function (str) {

​    var reg = new RegExp("^" + str);

​    return reg.test(this);

}

 

/*

\* 以某字符串结尾判断

*注意:参数str中如果包含正则通配字符,如"."请改参数为"\\."

*/

String.prototype.endWith = function (str) {

​    var reg = new RegExp(str + "$");

​    return reg.test(this);

}

 

/*

*字符串去除两侧空格或指定字符

*注意:参数str中如果包含正则通配字符,如"."请改参数为"\\."

*/

String.prototype.trimAll = function (str) {//js1.8版本以上自带trim,这里改名强烈建议不重复构写方法

​    str = (str ? str : "\\s");

​    str = ("(" + str + ")");

​    var reg_trim = new RegExp("(^" + str + "*)|(" + str + "*$)", "g");

​    return this.replace(reg_trim, "");

};

 

/*

*字符串去除左侧空格或指定字符

*注意:参数str中如果包含正则通配字符,如"."请改参数为"\\."

*/

String.prototype.trimLeft = function (str) {

​    str = (str ? str : "\\s");                            //没有传入参数的，默认去空格

​    str = ("(" + str + ")");

​    var reg_lTrim = new RegExp("^" + str + "*", "g");     //拼正则

​    return this.replace(reg_lTrim, "");

}

 

/*

*字符串去除右侧空格或指定字符

*注意:参数str中如果包含正则通配字符,如"."请改参数为"\\."

*/

String.prototype.trimRight = function (str) {

​    str = (str ? str : "\\s");

​    str = ("(" + str + ")");

​    var reg_rTrim = new RegExp(str + "*$", "g");

​    return this.replace(reg_rTrim, "");

}

 

/*

*字符串是否包含某字符串

*/

String.prototype.contains = function (str) {//利用indexof  

​    //值为位置,为-1说明不包含  

​    if (this.indexOf(str) >= 0) return true;

​    else return false;

};

 

/*

*字符串是否是大于0的整数或小数

*/

String.prototype.isPositiveDecimal = function () {

​    var reg = /^((0\.\d*[1-9])|([1-9]\d*(\.\d*[1-9])?))$/;

​    return reg.test(this);

}



//是否包含contains

 String.prototype.contains = function (str) {//利用indexof
            //值为位置,为-1说明不包含
            if (this.indexOf(str) >= 0) return true;
            else return false;
        };

```
    String.prototype.contains = function (str) {//利用正则表达式
        var reg = new RegExp(str);
        return reg.test(this);
    };
```

Array对象 是否包含某元素

```
 Array.prototype.contains = function(item){    for(i=0;i<this.length;i++){        if(this[i]==item){return true;}    }    return false;};
```

//获取字符数组
        String.prototype.toCharArray = function () {
            return this.split("");
        }
        //获取N个相同的字符串
        String.prototype.repeat = function (num) {
            var tmpArr = [];
            for (var i = 0; i < num; i++) {
                temArr.push(this);
                return temArr.join("");
            }
        }
        //逆序
        String.prototype.reverse = function () {
            return this.split("").reverse().join("");

```
    }
    //测试是否是数字
    String.prototype.isNumeric = function () {
        var tmpFloat = parseFloat(this);
        if (isNaN(tmpFloat))
            return false;
        var tmpLen = this.length - tmpFloat.toString().length;
        return tmpFloat + "0".Repeat(tmpLen) == this;
    }
    //测试是否是整数
    String.prototype.isInt = function () {
        if (this == "NaN")
            return false;
        return this == parseInt(this).toString();
    }
    // 合并多个空白为一个空白
    String.prototype.resetBlank = function () {
        return this.replace(/s+/g, " ");
    }
    // 除去左边空白
    String.prototype.LTrim = function () {
        return this.replace(/^s+/g, "");
    }
    // 除去右边空白
    String.prototype.RTrim = function () {
        return this.replace(/s+$/g, "");
    }
    // 除去两边空白
    String.prototype.trim = function () {
        return this.replace(/(^s+)|(s+$)/g, "");
    }
    // 保留数字
    String.prototype.getNum = function () {
        return this.replace(/[^d]/g, "");
    }
    // 保留字母
    String.prototype.getEn = function () {
        return this.replace(/[^A-Za-z]/g, "");
    }
    // 保留中文
    String.prototype.getCn = function () {
        return this.replace(/[^u4e00-u9fa5uf900-ufa2d]/g, "");
    }
    // 得到字节长度
    String.prototype.getRealLength = function () {
        return this.replace(/[^x00-xff]/g, "--").length;
    }
    // 从左截取指定长度的字串
    String.prototype.left = function (n) {
        return this.slice(0, n);
    }
    // 从右截取指定长度的字串
    String.prototype.right = function (n) {
        return this.slice(this.length - n);
    }
    // HTML编码
    String.prototype.HTMLEncode = function () {
        var re = this;
        var q1 = [/x26/g, /x3C/g, /x3E/g, /x20/g];
        var q2 = ["&", "<", ">", " "];
        for (var i = 0; i < q1.length; i++)
            re = re.replace(q1[i], q2[i]);
        return re;
    }
    // Unicode转化
    String.prototype.ascW = function () {
        var strText = "";
        for (var i = 0; i < this.length; i++)
            strText += "&#" + this.charCodeAt(i) + ";";
        return strText;
    }
    // 获取文件全名
    String.prototype.GetFileName = function () {
        var regEx = /^.*\/([^\/\?]*).*$/;
        return this.replace(regEx, '$1');
    };
    // 获取文件扩展名
    String.prototype.GetExtensionName = function () {
        var regEx = /^.*\/[^\/]*(\.[^\.\?]*).*$/;
        return this.replace(regEx, '$1');
    };
    //替换所有
    String.prototype.replaceAll = function (reallyDo, replaceWith, ignoreCase) {
        if (!RegExp.prototype.isPrototypeOf(reallyDo)) {
            return this.replace(new RegExp(reallyDo, (ignoreCase ? "gi" : "g")), replaceWith);
        } else {
            return this.replace(reallyDo, replaceWith);
        }
    };
    //格式化字符串
    String.prototype.Format = function () {
        if (arguments.length == 0) {
            return '';
        }
        if (arguments.length == 1) {
            return arguments[0];
        }
        var reg = /{(\d+)?}/g;
        var args = arguments;
        var result = arguments[0].replace(reg, function ($0, $1) {
            return args[parseInt($1) + 1];
        });
        return result;
    };
 
    // 数字补零
    Number.prototype.LenWithZero = function (oCount) {
        var strText = this.toString();
        while (strText.length < oCount) {
            strText = '0' + strText;
        }
        return strText;
    };
    // Unicode还原
    Number.prototype.ChrW = function () {
        return String.fromCharCode(this);
    };
```

 

```
    // 数字数组由小到大排序
    Array.prototype.Min2Max = function () {
        var oValue;
        for (var i = 0; i < this.length; i++) {
            for (var j = 0; j <= i; j++) {
                if (this[i] < this[j]) {
                    oValue = this[i];
                    this[i] = this[j];
                    this[j] = oValue;
                }
            }
        }
        return this;
    };
 
    // 数字数组由大到小排序
    Array.prototype.Max2Min = function () {
        var oValue;
        for (var i = 0; i < this.length; i++) {
            for (var j = 0; j <= i; j++) {
                if (this[i] > this[j]) {
                    oValue = this[i];
                    this[i] = this[j];
                    this[j] = oValue;
                }
            }
        }
        return this;
    };
 
    // 获得数字数组中最大项
    Array.prototype.GetMax = function () {
        var oValue = 0;
        for (var i = 0; i < this.length; i++) {
            if (this[i] > oValue) {
                oValue = this[i];
            }
        }
        return oValue;
    };
 
    // 获得数字数组中最小项
    Array.prototype.GetMin = function () {
        var oValue = 0;
        for (var i = 0; i < this.length; i++) {
            if (this[i] < oValue) {
                oValue = this[i];
            }
        }
        return oValue;
    };
 
    Array.prototype.add = function (item) {
        this.push(item);
    }
    Array.prototype.addRange = function (items) {
        var length = items.length;
        if (length != 0) {
            for (var index = 0; index < length; index++) {
                this.push(items[index]);
 
            }
        }
    }
    Array.prototype.clear = function () {
        if (this.length > 0) {
            this.splice(0, this.length);
        }
    }
    Array.prototype.isEmpty = function () {
        if (this.length == 0) {
            return true;
        }
        else {
            return false;
        }
    }
    Array.prototype.clone = function () {
        var clonedArray = [];
        var length = this.length;
        for (var index = 0; index < length; index++) {
            clonedArray[index] = this[index];
        }
        return clonedArray;
    }
    Array.prototype.contains = function (item) {
        var index = this.indexOf(item);
        return (index >= 0);
    }
    Array.prototype.dequeue = function () {
        return this.shift();
    }
    Array.prototype.indexOf = function (item) {
        var length = this.length;
 
        if (length != 0) {
            for (var index = 0; index < length; index++) {
                if (this[index] == item) {
                    return index;
                }
            }
        }
        return -1;
    }
    Array.prototype.insert = function (index, item) {
        this.splice(index, 0, item);
    }
    Array.prototype.joinstr = function (str) {
        var newStr = new Array(this.length);
        for (var i = 0; i < this.length; i++) {
            newStr[i] = this[i] + str;
        }
        return newStr;
    }
    Array.prototype.queue = function (item) {//入队
        this.push(item);
    }
    Array.prototype.remove = function (item) {
        var index = this.indexOf(item);
        if (index >= 0) {
            this.splice(index, 1);
        }
    }
    Array.prototype.removeAt = function (index) {
        this.splice(index, 1);
    }
    //给js原生Array增加each方法
    Array.prototype.each = function (fn) {
        return this.length ? [fn(this.slice(0, 1))].concat(this.slice(1).each(fn)) : [];
    };
 
    // 获取当前时间的中文形式
    Date.prototype.GetCNDate = function () {
        var oDateText = '';
        oDateText += this.getFullYear().LenWithZero(4) + new Number(24180).ChrW();
        oDateText += this.getMonth().LenWithZero(2) + new Number(26376).ChrW();
        oDateText += this.getDate().LenWithZero(2) + new Number(26085).ChrW();
        oDateText += this.getHours().LenWithZero(2) + new Number(26102).ChrW();
        oDateText += this.getMinutes().LenWithZero(2) + new Number(20998).ChrW();
        oDateText += this.getSeconds().LenWithZero(2) + new Number(31186).ChrW();
        oDateText += new Number(32).ChrW() + new Number(32).ChrW() + new Number(26143).ChrW() + new Number(26399).ChrW() + new String('26085199682010819977222352011620845').substr(this.getDay() * 5, 5).ToInt().ChrW();
        return oDateText;
    };
    //扩展Date格式化
    Date.prototype.Format = function (format) {
        var o = {
            "M+": this.getMonth() + 1, //月份
            "d+": this.getDate(), //日
            "h+": this.getHours() % 12 == 0 ? 12 : this.getHours() % 12, //小时
            "H+": this.getHours(), //小时
            "m+": this.getMinutes(), //分
            "s+": this.getSeconds(), //秒
            "q+": Math.floor((this.getMonth() + 3) / 3), //季度
            "S": this.getMilliseconds() //毫秒
        };
        var week = {
            "0": "\u65e5",
            "1": "\u4e00",
            "2": "\u4e8c",
            "3": "\u4e09",
            "4": "\u56db",
            "5": "\u4e94",
            "6": "\u516d"
        };
        if (/(y+)/.test(format)) {
            format = format.replace(RegExp.$1, (this.getFullYear() + "").substr(4 - RegExp.$1.length));
        }
        if (/(E+)/.test(format)) {
            format = format.replace(RegExp.$1, ((RegExp.$1.length > 1) ? (RegExp.$1.length > 2 ? "\u661f\u671f" : "\u5468") : "") + week[this.getDay() + ""]);
        }
        for (var k in o) {
            if (new RegExp("(" + k + ")").test(format)) {
                format = format.replace(RegExp.$1, (RegExp.$1.length == 1) ? (o[k]) : (("00" + o[k]).substr(("" + o[k]).length)));
            }
        }
        return format;
    }
    Date.prototype.Diff = function (interval, objDate) {
        //若参数不足或 objDate 不是日期类型則回传 undefined
        if (arguments.length < 2 || objDate.constructor != Date) { return undefined; }
        switch (interval) {
            //计算秒差
            case 's': return parseInt((objDate - this) / 1000);
                //计算分差
            case 'n': return parseInt((objDate - this) / 60000);
                //计算時差
            case 'h': return parseInt((objDate - this) / 3600000);
                //计算日差
            case 'd': return parseInt((objDate - this) / 86400000);
                //计算周差
            case 'w': return parseInt((objDate - this) / (86400000 * 7));
                //计算月差
            case 'm': return (objDate.getMonth() + 1) + ((objDate.getFullYear() - this.getFullYear()) * 12) - (this.getMonth() + 1);
                //计算年差
            case 'y': return objDate.getFullYear() - this.getFullYear();
                //输入有误
            default: return undefined;
        }
    };
 
    //检测是否为空
    Object.prototype.IsNullOrEmpty = function () {
        var obj = this;
        var flag = false;
        if (obj == null || obj == undefined || typeof (obj) == 'undefined' || obj == '') {
            flag = true;
        } else if (typeof (obj) == 'string') {
            obj = obj.trim();
            if (obj == '') {//为空
                flag = true;
            } else {//不为空
                obj = obj.toUpperCase();
                if (obj == 'NULL' || obj == 'UNDEFINED' || obj == '{}') {
                    flag = true;
                }
            }
        }
        else {
            flag = false;
        }
        return flag;
    }
```

