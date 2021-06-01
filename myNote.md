5/29~5/30 Node.js_Note

---------------------------------------------------
### 第二次測驗小筆記 ###

何謂效能好壞? 並無絕對   
改善網站效能你需要先了解現況!(對方需求是甚麼、目標要甚麼樣子),   
優化前的第一步：了解針對效能定義、基準點在哪,我們可以按照情境去優化   
同步與非同步執行速度誰快? 同步比較快(平均來說,但不一定歐!)   
php靠很多Process來做   
讀檔案: 同步的nodejs比較快   
溝通工作有代價   
代價為是否阻塞下一行程式碼   


## 甚麼是XHR?   

XMLHttpRquest(瀏覽器物件) 發請求,三個參數 Method 網址列 同步/非同步:True/False    
```javascript
function reqListener () {   
  console.log(this.responseText);   
}   
var oReq = new XMLHttpRequest();   
oReq.addEventListener("load", reqListener);   
oReq.open("GET", "http://www.example.org/example.txt");   
oReq.send();   
```
(或是也可以用.onload)   

## 為什麼要有同源政策(CORS)?   

主要安全性考量,例如來自於不同網域（domain）、通訊協定（protocol）或通訊埠（port）的資源時，會建立一個跨來源 HTTP 請求（cross-origin HTTP request）。   
參考資料:   
- [CORS跨來源資源共用](https://developer.mozilla.org/zh-TW/docs/Web/HTTP/CORS)

其他:   

Jquery fetch 都是非同步 (底層:就是XMLHttpRquest)   
但也可以用同步(但會阻塞)   
需要長時間工作的包裝成非同步,牽扯到io讀取檔案或網路請求對cpu來說都是比較慢的事情   
可能會阻塞(同步請求會造成阻塞)   
./adapters/xhr也是用XMLHttpRequest   

---------------------------------------------------   

# callback:   
延續傳遞風格(Continuation-passing style, CPS)   
基本上一個程式語言要具有高階函式(High Order Function)的特性才能使用CPS風格，   
也就是可以把某個函式當作另一函式的傳入參數，也可以回傳函式。除了JavaScript語言外，   
具有高階函式特性的程式語言常見的有Python、Java、Ruby、Swift等等   

非同步回調函式-   
使用計時器(timer)函式: setTimeout, setInterval   
特殊的函式: nextTick, setImmediate   
執行I/O: 監聽網路、資料庫查詢或讀寫外部資源   
訂閱事件   

# 回調地獄(Callback Hell)   
複雜的情況是在於CPS風格使用callback(回調)來移往下一個函式執行，   
當你開始撰寫一個接著一個執行的流程，   
也就是一個特定工作的函式呼叫後要接下一個特定工作的函式時，   
就會看到所謂的"回調地獄"的結構   

callback(回調)的函式名稱，可以用匿名函式取代。(實際上callback的名稱在除錯時很有用，可以在錯誤的堆疊上指示出來)   
callback(回調)因為是函式的定義，所以傳入參數value的名稱可以自己取。   
callback(回調)其實有Closure(閉包)結構的特性，可以獲取到func中的傳入參數，以及裡面的定義的值。   
(實際上JavaScript中只要函式建立就會有閉包產生)   

---------------------------------------------------

# axios:   
Axios是個基於promise的HTTP,可以用在browser & node.js   
從瀏覽器創建XMLHttpRequests   
可轉換json資料格式   
若無指定method,請求會使用get(default)   
語法:
```javascript   
axios.get('URL')   
.then(function (response) {   
console.log(response);   
})   
.catch(function (error) {   
console.log(error);   
});   
```
或是也可寫成下面形式   

```javascript
axios.get('URL', {   
params: {   
	 response: "json",   
     date: moment,   
     stockNo: data,   
}   
})   
.then(function (response) {   
console.log(response);   
})   
.catch(function (error) {   
console.log(error);   
});   
```  
參考資料:    
- [Axios的使用說明](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/691120/)
 

---------------------------------------------------   

# Promise:    
pending/resolve/rejected   
Promise最大的好處是在非同步執行的流程中，有物件就可以串接.then.catch   
把執行代碼和處理結果的代碼清晰地分離   

-promise重點是物件 所以要new一個物件   
"最終" "完成或失敗" 的 "物件"   

參數固定 function resolve,reject    
參數名稱(resolve,reject)也可自行命名 不過還是用常規寫法   
本質上還是非同步 但程式可以依序進行   
解決回調地獄（Callback Hell）問題   
基本語法-> new Promise(function (resolve, reject) {});   

---------------------------------------------------

# Async/await:   
await 也能夠把 Promise 回傳的值接起來，   
通常我們在呼叫 API（例如執行 fetch、axios）   
搭配 axios 更可以這樣使用：      
```javascript
((async () => {   
    const { data } = await axios.get('API_URL');   
    console.log(data);   
})();
```      
使用 async/await 呼叫 API 或是其他非同步方法，   
不但可以避免 Callback Hell，   
比起 Promise 更增加了程式可讀性   

---------------------------------------------------

補充:   
何謂php & javascript物件導向   
class -> instance   
obj = new class 拿模子做出個吐司   
prototype原型本身就是個物件   


