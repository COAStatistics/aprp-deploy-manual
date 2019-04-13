# Django專案部屬到IIS流程

## 環境

*   IIS 8
*   Python 3.6
*   Django 1.9

## 在本機端測試網站

1.  將專案`repo`下載至 C:\inetpub\wwwroot 底下
2.  在命令提示字元中將路徑切換到網站資料夾 `cd C:\inetpub\wwwroot\repo`
3.  `pip install -r requirements.txt` 安裝所需的套件
4.  `cd src` 將路徑切換到 src，並執行 `python manage.py runserver`
5.  在瀏覽器開啟 [http://localhost:8000](http://localhost:8000) 檢視網站

## 設定 IIS 環境

### FastCGI

1.  打開 IIS manager，在左邊清點選取主機，然後在右邊的選單找到 FastCGI
2.  在右側點選 `新增應用程式`
3.  執行檔為 `python.exe` 位置如：`<python安裝路徑>\python.exe` 引數為 `wfastcgi.py`如：`<python安裝路徑>\lib\site-packages\wfastcgi.py`
4.  點選 `一般>環境變數`，新增參數
5.  名稱 `DJANGO_SETTINGS_MODULE` 值 `dashboard.settings`
6.  名稱 `PYTHONPATH` 值 `C:\inetpub\wwwroot\repo\src`
7.  名稱 `WSGI_HANDLER` 值 `django.core.wsgi.get_wsgi_application()`

#### Note  

可能會需要額外安裝`wfastcgi==3.0.0`及`whitenoise==3.3.1`

### 新增網站至 IIS

1.  在現有站台上點右鍵 `新增網站`
2.  輸入名稱，實體路徑 `C:\inetpub\wwwroot\repo\src`
3.  網站標頭 `網站的網址`

### 模組對應

1.  左側清單選取網站，在選單中找到 `處理常式對應`，右邊點擊 `新增模組對應`
2.  要求路徑： `*`，模組：`FastCgiModule`
3.  執行檔：`<python安裝路徑>\python.exe|<python安裝路徑>\lib\site-packages\wfastcgi.py`
4.  名稱：`Django Handler`（或是隨意）
5.  要求限制 -> 取消勾選 `只有當要求對應到下列項目時才啟動處理常式`

### 權限設定

1.  在 python 安裝資料夾點右鍵 `內容>安全性`，編輯使用者權限，新增 `IIS_IUSRS` 並套用

## 參考網址

*   [https://randomdize.github.io/2017/07/12/django-on-iis/](https://randomdize.github.io/2017/07/12/django-on-iis/)
*   [http://blog.mattwoodward.com/2016/07/running-django-application-on-windows.html](http://blog.mattwoodward.com/2016/07/running-django-application-on-windows.html)
