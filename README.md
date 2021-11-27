# LineStockQuoter
* 這是一個用 Django, Line API 架設並部署在 Heroku 的Line聊天機器人。

![home](/static/githubImages/4.PNG)

## 功能
* 響應式(RWD)網站設計
* 首頁圖片輪播(Carousel)
* 首頁商品導覽換頁(Paginator)
* 使用者登入驗證與註冊(django auth)
* 第三方登入驗證(Google、GitHub)
* 利用URL patterns增刪購物車內容(Restful API)
* 將商品加入購物車(django-shopping-cart)並提交訂單(session)
* 發送訂單確認的電子郵件給顧客(SMTP + Gmail)
* Admin管理後臺可以上傳產品圖片(django-filer)

## 使用技術與工具
* 前端:
    - HTML5
    - CSS3
    - jquery
    - [Bootstrap(4.5.2)](https://getbootstrap.com/)
* 後端:
    - [Django(3.1.7)](https://www.djangoproject.com/)
        - session
        - form
        - email(SMTP+Gmail)
        - django-allauth(Google、GitHub)  
        - django-shopping-cart
        - djagno-filer
* 資料庫:
    - [MySQL](https://www.mysql.com/)
    - [PostgreSQL(Heroku)](https://www.postgresql.org/)
    - [SQLite](https://www.sqlite.org/index.html)
* 部署:
    - [Docker](https://www.docker.com/)
    - [Heroku](https://dashboard.heroku.com/)

## 專案上遇到的問題
### Config組態設定
* 因為部署到 github 會有 `client_secret` 外露的資安問題，所以編輯一個 `config.ini` 的組態設定檔，內含各種密碼，透過 `configer` 的讀取方式，避免暴露密碼，也方便統一管理所需要的參數

### Django套件
* `django-shopping-cart` 內建的 `add` 函式無法存取 product，導致 `OrderItem` 這個 model 無法跟 `Product` 這個 model 以 foreign key 做關聯，地端程式碼可以手動修改，但部署到 Heroku 就無法修改套件py檔內容，因此最後 Heroku app 取消關聯到 `Product` 這個資料表
* `python manage.py makemigrations` 若失效，要進到 `migrations` 資料夾，手動查找每個版本的py檔，找出與model修正有關聯的檔案，手動修正或刪除，重新執行此指令即可成功運行

### Heroku部署
* 本機作業環境是 Windows，利用 `pip freeze > requirements.txt` 指令會生成 `pywin300`這個套件，但Heroku是Linux環境，所以會部署失敗，需要手動刪除此套件
* Heroku 預設的資料庫是 PostgreSQL，因此 `settings` 裏頭要修改DB連結的方式，才能在 Heroku 上部署成功
* Heroku 部署需要提供三個檔案，`Procfile`、`requirements.txt`、`runtime.txt`