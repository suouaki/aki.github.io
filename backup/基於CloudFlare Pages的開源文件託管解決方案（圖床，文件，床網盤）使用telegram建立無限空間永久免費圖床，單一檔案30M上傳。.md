>覆蓋大多數文件格式：支援絕大多數常見圖片、影片、動圖等，同時也支援其他大多數格式的文件
>免費文件存放解決方案，具備上傳、管理、讀取、刪除等全貨架功能，涵蓋文件全生命週期，支援鑑權、目錄、圖片審查、隨機圖等各項各項功能
![示例图片](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-11%20224954.png)
---
準備工作
------
- [githube](https://github.com/)帳號
- [cloudflare](https://dash.cloudflare.com/)帳號
- Telegram帳號

前提
------
 - 創建BotBotbot机器人 [@BotFather](https://t.me/BotFather)發送/newbot，依照提示輸入機器人的備註、使用者名稱等資訊。成功創建後獲得TG_BOT_TOKEN。
   -  建立一個新的頻道（Channel），進入新建的頻道，選擇頻道管理，將剛才建立的機器人設為頻道管理員。
   -  向[@VersaToolsBot](https://t.me/VersaToolsBot)轉發一則第2步驟新頻道中的消息，獲取TG_CHAT_ID（ID）
 ![示例图片](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-11%20225632.png)
---
開始部署
------
1.在github，Fork[MarSeventh/CloudFlare-ImgBed：基於CloudFlare Pages的開源文件託管解決方案（圖床/文件床/網盤）](https://github.com/MarSeventh/CloudFlare-ImgBed)
2.使用Cloudflare
 - 配置Pages项目
 - 选择Cloudflareboard倉庫
3.随意填寫項目名稱，建構指令填写
```建構指令
npm install
``` 
4.配置环境变量

```python
AUTH_CODE = aaaaaaa #登入密码
BASIC_USER = aaaaaa #管理名称
BASIC_PASS = aaaaaa #管理密码
TG_CHAT_ID = -1002180544284 #频道ID
TG_BOT_TOKEN = 1544711526:AAHiQI #機器人API
``` 
![示例图片](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-11%20225327.png)

5.綁定KV資料庫：
   1,建立一個新的KV資料庫，名称随意。
   2,進入對應項目設定->綁定->新增->KV命名空間->變數名稱，填入img_url，KV命名空間選擇剛才建立好的KV資料庫
6.重新部署pages项目
  1.点击箭头所指的地方*重试部署*
  2.访问cloudflare分配的地址访问图床
![示例](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-11%20230248.png)

*访问链接过长可以绑定自己域名进行访问*

完成搭建
------
![示例](http://momo-1-img.ao1160301aila.workers.dev/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202025-04-11%20230011.png)