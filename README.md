# Final Project: 台股即時資訊&LineBot應用
## Introduction
最近有小小研究股市，但每次看盤或想查詢一支股票的時候都得打開app，
就想到可以透過證交所的API(twstock)，結合Linebot，只要傳送想看的股票代碼，
就能及時回傳資訊。
## Build Process

* 首先先下載pakage，就可以開始對twstock操作

```pip install twstock```

```import twstock```
* 有許多function可以做使用(詳見twstock資料夾 or [twstock API Reference](https://twstock.readthedocs.io/zh_TW/latest/reference/stock.html))
* 載入matplotlib畫出走勢圖

```pip install matplotlib``` 

```import matplotlib.pyplot as plt```

* LineBot

先進入Line development建立provider，建立message api channel，就可以創建出Linebot，
但要讓Linebot回傳訊息，需要安裝Linebot SDK才能在程式中加入Linebot api。

```pip install line-bot-sdk==1.8.0```

然後使用flask建立網站伺服器，設定Channel secret及Channel access token，
並用line_bot_api.reply_message就可以用程式回傳想回傳的訊息。

在這之前，我們需要一個代理伺服器，我使用的是ngrok。

我參考的教學是這個。[6步驟快速上手LINE Bot機器人](https://www.learncodewithmike.com/2020/06/python-line-bot.html)
## Details of the Approach
經過測試，最後穩定輸出的是可以查詢股票的基本資料，近五日資訊(包括開盤價、最高點、最低點)，和股票的即時資料。
而走勢圖有用matplotlib在程式中畫出來，但是回傳LineBot還未成功。

這個api是抓取台灣證券交易所網站的資料，缺點是這個網站有防爬蟲的機制，只要在時間內(幾秒內)存取資料太多次，這個IP就會被Ban掉，
這困擾了我很久，最後找到唯一一個辦法，設sleep，經過測試，每抓取一次sleep 3秒是最保險的，但這又減少了能獲得即時資料的優點
因為搓合是在3秒內會完成，加上linebot可能會delay一兩秒，可能抓到的資料就不是即時資料了。

## References
* API: twstock、Flask、LINE Bot、Imgur
* Data: [台灣證券交易所](https://www.twse.com.tw/zh/)
* linebot教學: [6步驟快速上手LINE Bot機器人](https://www.learncodewithmike.com/2020/06/python-line-bot.html)
## Results
* 股票基本資料、近五日、即時資料

![image](https://user-images.githubusercontent.com/62441311/122953511-5a255400-d3b1-11eb-89a7-baaee6ae3a8f.png)
![image](https://user-images.githubusercontent.com/62441311/122953957-9c4e9580-d3b1-11eb-827b-a16819cb897b.png)

* 輸入錯誤時會回傳"請重新輸入"

![image](https://user-images.githubusercontent.com/62441311/122954301-e46db800-d3b1-11eb-81e5-75a79549d400.png)

