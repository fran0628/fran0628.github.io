### This is Fran0628's blog 
## md = markdown 
# = html h1 
## = html h2

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        README.md 
沒被加到repo

0523 nodejs影片NOTE--------我是分隔線

# note.js----------note

專有名詞:
call stack(一種資料結構:堆疊(後進先出) /stack webapis / callback task queue(佇列) 
event loop (事件循環)

JavaScript:
(one thread = one call stack = one job at one time)
單執行緒語言 簡單來說就是一次只能做一件事(一段code會由上到下執行下去)

-setTimeout設定0或5秒,執行上的差別

stack執行setTimeout;丟到webapis上並啟動計時器
倒數結束後不會直接回到stack,避免stack會隨機執行(並顯示在browser畫面上)
因此task queue就發揮作用 (任何webapis都會被回調推送到task queue)
最後回到事件循環(event loop,事件循環特性:得等到stack清空後才會callback;就能接著繼續執行)
event loop就是查看stack 並查看task queue
查看結果發現stack是空的 就會把task queue內的code傳回stack 
也就是js領域 (回到V8)接著執行 console.log("") -> DONE

結論:
如影片提到 重點是event loop(事件循環) 感覺有點像是協調者或監工(?想不到更好的表達)
而它的任務就是確認task queue是否還有要執行的工作 
若沒有就會將task queue以call back傳到stack去執行

其他: 
1.事件循環 
js runtime 一次只能做一件事
執行緒是讓C++ API隱藏
(使用的是C++ API 不是WebAPI)

2.所有webapis都是以上述方式運作 (EX: ajax URL請求 

3.rerender 阻塞 
今日上課提到用php跑壓力測試 (10000個封包/1000個使用者)同步請求 瀏覽器無法運作 
-> 解決方式: Async Callback 非同步回調

