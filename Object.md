```
/**
 * Date对象方法扩展
 */
Object.assign(Date.prototype,{
    // 对Date的扩展，将 Date 转化为指定格式的String
    // 月(M)、日(d)、小时(h)、分(m)、秒(s)、季度(q) 可以用 1-2 个占位符，
    // 年(y)可以用 1-4 个占位符，毫秒(S)只能用 1 个占位符(是 1-3 位的数字)
    // 例子：
    // (new Date()).Format("yyyy-MM-dd hh:mm:ss.S") ==> 2006-07-02 08:09:04.423
    // (new Date()).Format("yyyy-M-d h:m:s.S")      ==> 2006-7-2 8:9:4.18
    // var time1 = new Date().Format("yyyy-MM-dd hh:mm:ss");
    // var time2 = new Date().Format("yyyy-MM-dd");
    // var time3 = new Date("2012-12-12").Format("yyyy-MM-dd hh:mm:ss");
    Format: function(fmt) {
        var o = {
            "M+" : this.getMonth()+1,                 //月份
            "d+" : this.getDate(),                    //日
            "h+" : this.getHours(),                   //小时
            "m+" : this.getMinutes(),                 //分
            "s+" : this.getSeconds(),                 //秒
            "q+" : Math.floor((this.getMonth()+3)/3), //季度
            "S"  : this.getMilliseconds()             //毫秒
        };
        if(/(y+)/.test(fmt))
            fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length));
        for(var k in o)
            if(new RegExp("("+ k +")").test(fmt))
                fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
        return fmt;
    }
});
```

```
/**
 * 公共方法 使用方式 window.util.方法
 */

export const util = {
    
    /**
     * 判断参数是否为null、undefined、""、" "、字符串null、字符串undefined、字符串{}；(除去0的情况)
     */
    isEmpty:function(str) {
        var typeStr = typeof (str);
        if (str === "" || str == null || str == undefined || typeStr == 'undefined') {
            return true;
        } else if(typeStr=="string"){
            var spaceRe = new RegExp("^[ ]+$");
            //为空
            if(spaceRe.test(str)){
                return true;
            }else {
                str = str.toUpperCase();//转为大写
                return (str=="NULL" || str=="UNDEFINED" || str=="{}");
            }
        }
        return false;
    },
    
   
   /**
     * 判断srt 是否有汉字
     */
    checkInChinese: function(str) {
        var reg = new RegExp("[\\u4E00-\\u9FFF]+","g");
        return reg.test(str);
    },
    
    
    /**
     * 获取URL上的参数
     * keyName: url的key值，为空或者传""表示返回参数组成的对象
     * url: url链接，为空的话用window.location.href代替
     * 比如getUrlParam(),返回{key:"1",id:"2"}。(假设window.location.href=www.baidu.com?key=1&id=2)
     */
    getUrlParam: function(keyName,urlParam) {
        var urlName = urlParam ? urlParam : window.location.search;
        var arr,paramJson={};
        if(urlName==""){
            return null;
        }else {
            urlName = decodeURI(urlName);
            var paramStr=urlName.split("?")[1];
            if(paramStr==undefined) return null;
            var items = paramStr.split("&");
            for(var i= 0,l=items.length;i<l;i++){
                arr=items[i].split("=");
                paramJson[arr[0]]=arr[1];
            }
        }
        if(keyName){
            if(paramJson[keyName]!=undefined){
                return paramJson[keyName];
            }else {//不存在这个key值
                return null;
            }
        }else {
            return paramJson;
        }
    },
    
    /**
     * 获取格式化时间（根据时间戳）
     * 例：$.formatTime(112132312,"yyyy/MM/dd")
     * @param time 时间戳
     * @param fmt  时间格式
     * @returns {*}
     */

    formatTime: function(time, fmt) {
        if(this.isEmpty(time)){
            return "";
        }
        if(this.isEmpty(fmt)){
            fmt = "yyyy-MM-dd";
        }
        return new Date(time).Format(fmt);
    },
    
   
   /**
     * 获取月份的最后一天是几号
     * 例：$.getLastDay(2013,11)
     * @param year
     * @param month
     * @returns {number}
     */
    getLastDay: function(year,month) {
        var new_year = year;    //取当前的年份
        var new_month = month++;//取下一个月的第一天，方便计算（最后一天不固定）
        if(month>12)            //如果当前大于12月，则年份转到下一年
        {
            new_month -=12;        //月份减
            new_year++;            //年份增
        }
        var new_date = new Date(new_year,new_month,1);                //取当年当月中的第一天
        return (new Date(new_date.getTime()-1000*60*60*24)).getDate();//获取当月的天数
    },
    
    
    /**
     *  判断手机号码是否是合理的
     *  验证规则符合：138../145../147../149../15..(除了154.)/16../17..(除了172./174./179.)/198../199..（长度11位）
     * @param num 要判断的手机号码
     * @returns {boolean} true 代表合理  false 代表不合理
     */
    isPhoneNum(num){
        var phoneReg = /^1([38][0-9]|4[579]|5[0-3,5-9]|6[6]|7[0135678]|9[89])\d{8}$/;
        if (!phoneReg.test(num)){
            return false;
        }else {
            return true;
        }
    },
   
   
   
   /**
     *  判断密码是否是合理的
     *  验证规则符合：以字母开头，且6-20位的密码
     * @param value 要判断的密码
     * @returns {boolean} true 代表合理  false 代表不合理
     */
    isPassWord(value){
        var PassReg = /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,20}$/;
        if (!PassReg.test(value)){
            return false;
        }else {
            return true;
        }
    },
   
   
   /**
     * 是否为合法身份证
     * @param idCard: 要验证的身份证号码
     * 返回 "0"=身份证为合法的，"1"=身份证号码位数不对，"2"=身份证号码出生日期超出范围或含有非法字符，
     * "3"=身份证号码校验错误 "4"=身份证号码不能为空
     * @return Object
     */
    isCardNo: function(idCard) {
        var Errors=[{type:"0",message:"身份证为合法"},{type:"1",message:"身份证号码位数不对"},{type:"2",message:"出生日期超出范围或含有非法字符"},
            {type:"3",message:"身份证号码校验错误"},{type:"4",message:"身份证号码不能为空"}];
        if(!(""+idCard)) return Errors[4];//为空 返回4
        var area={11:"",12:"",13:"",14:"",15:"",21:"",22:"",23:"",31:"",32:"",33:"",34:"",35:"",36:"",37:"",41:"",42:"",43:"",44:"",45:"",46:"",50:"",51:"",52:"",53:"",54:"",61:"",62:"",63:"",64:"",65:"",71:"",81:"",82:"",91:""}

        var Y,JYM;
        var S, M,ereg;
        var idCard_array = idCard.split("");
        //地区检验
        if(area[parseInt(idCard.substr(0,2))]==null){
            return Errors[3];
        }
        //身份号码位数及格式检验
        switch(idCard.length){
            case 15:
                if( (parseInt(idCard.substr(6,2))+1900) % 4 == 0 || ((parseInt(idCard.substr(6,2))+1900) % 100 == 0 && (parseInt(idCard.substr(6,2))+1900) % 4 == 0 )){
                    ereg=/^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}$/;//测试出生日期的合法性
                } else {
                    ereg=/^[1-9][0-9]{5}[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}$/;//测试出生日期的合法性
                }
                if(ereg.test(idCard)){
                    return Errors[0];
                } else {
                    return Errors[2];
                }
                break;
            case 18:
                //18位身份号码检测
                //出生日期的合法性检查
                //闰年月日:((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))
                //平年月日:((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))
                if ( parseInt(idCard.substr(6,4)) % 4 == 0 || (parseInt(idCard.substr(6,4)) % 100 == 0 && parseInt(idCard.substr(6,4))%4 == 0 )){
                    ereg=/^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|[1-2][0-9]))[0-9]{3}[0-9Xx]$/;//闰年出生日期的合法性正则表达式
                } else {
                    ereg=/^[1-9][0-9]{5}19[0-9]{2}((01|03|05|07|08|10|12)(0[1-9]|[1-2][0-9]|3[0-1])|(04|06|09|11)(0[1-9]|[1-2][0-9]|30)|02(0[1-9]|1[0-9]|2[0-8]))[0-9]{3}[0-9Xx]$/;//平年出生日期的合法性正则表达式
                }
                if(ereg.test(idCard)){//测试出生日期的合法性
                    //计算校验位
                    S = (parseInt(idCard_array[0]) + parseInt(idCard_array[10])) * 7
                        + (parseInt(idCard_array[1]) + parseInt(idCard_array[11])) * 9
                        + (parseInt(idCard_array[2]) + parseInt(idCard_array[12])) * 10
                        + (parseInt(idCard_array[3]) + parseInt(idCard_array[13])) * 5
                        + (parseInt(idCard_array[4]) + parseInt(idCard_array[14])) * 8
                        + (parseInt(idCard_array[5]) + parseInt(idCard_array[15])) * 4
                        + (parseInt(idCard_array[6]) + parseInt(idCard_array[16])) * 2
                        + parseInt(idCard_array[7])
                        + parseInt(idCard_array[8]) * 6
                        + parseInt(idCard_array[9]) * 3;
                    Y = S % 11;
                    M = "F";
                    JYM = "10X98765432";
                    M = JYM.substr(Y,1);//判断校验位
                    if(M == idCard_array[17]) {
                        return Errors[0];   //检测ID的校验位
                    } else{
                        return Errors[3];
                    }
                } else{
                    return Errors[2];
                }
                break;
            default:
                return Errors[1];
                break;
        }

    },

    
    /**
     * 取 date2 与 date1： 天数之差   date2大于 date1 取正数 反之 负数（此处没有比较时分秒 直接比较年月日）
     * @param date1
     * @param date2
     * @returns {*}
     */
    getDateDiff(date1,date2){
        //有一个不是时间就 return false
        if(!this.isDate(date1) || !this.isDate(date2)) return false;
        var t1 = new Date(date1);
        var t2 = new Date(date2);

        t1.setHours(0,0,0);
        t2.setHours(0,0,0);

        var dateTime = 1000*60*60*24; //每一天的毫秒数
        var minusDays = Math.floor(((t2.getTime()-t1.getTime())/dateTime));//计算出两个日期的天数差
        return minusDays;
    },
  
  
  
  /**
   * 验证是否是时间 如： new Date()、时间戳、2017/1/1 10:16:16
   * @param date
   * @returns {boolean} true:是时间  false:不是时间
   */
    isDate(date){
        if (date === null || date === undefined) return false;
        if (isNaN(new Date(date).getTime())) return false;
        return true;
    },
    
    
    /**
     * 防抖
     * @param fn
     * @param wait
     * @returns {Function}
     */
    debounce(fn, wait) {
        var timeout = null;
        return function() {
            if(timeout !== null) clearTimeout(timeout);
            timeout = setTimeout(fn, wait);
        }
    }

};
```