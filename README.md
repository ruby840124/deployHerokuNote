# deployHerokuNote
之前有將React app部署到heroku，<br>
故整理一下筆記~ <br>
總共有三個步驟:<br>
**第一步驟是創建一個react app** <br>
**第二步驟是創建一個heroku應用程式** <br>
**第三步驟是將創建的app部署至heroku** <br>

## 1.創建一個react app <br>
`為什麼要使用react app?`<br>
React app為臉書開發的快速建置專案環境的工具。<br>
在自己建置react專案時，<br>
需要babel轉譯語法，<br>
且還需要用webpack打包程式碼，<br>
有了react app可以輸入幾行指令立刻幫你打包這些react所需的環境。<br><br>

在cmd打以下指令: <br>
```js     
npx create-react-app my-app //創建名叫my-app的react app專案
cd my-app //進入至my-app資料夾
npm start //run 專案
```
下指令npx create-react-app my-app成功專案畫面<br>
(my-app可以換成你想要的專案名稱)<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/1.png" width="60%" height="60%"><br><br>
下指令npm start，跑起第一個react專案<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/2.png" width="60%" height="60%"><br><br>
此為my-app的資料夾內容，可以看到相關套件都已經安裝完畢<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/3.png" width="60%" height="60%"><br><br>
為了讓react app專案在heroku成功run，<br>
還需要額外一些動作，<br>
首先創建一個express服務，<br>
之後可以在heroku run你的程式碼，新增**server.js**，並在server.js打入以下程式碼:<br>
```js     
//server.js
const express = require('express');
const favicon = require('express-favicon');
const path = require('path');
const port = process.env.PORT || 8080;
const app = express();
app.use(favicon(__dirname + '/build/favicon.ico'));
// the __dirname is the current directory from where the script is running
app.use(express.static(__dirname));
app.use(express.static(path.join(__dirname, 'build')));
app.get('/ping', function (req, res) {
 return res.send('pong');
});
app.get('/*', function (req, res) {
  res.sendFile(path.join(__dirname, 'build', 'index.html'));
});
app.listen(port);
```
接下來安裝express、express-favicon、path套件，同樣在cmd輸入以下指令:<br>
```js     
yarn add express express-favicon path
```
並將package.json file的start改成<br>
```js     
"start": "node server.js"
```
接著如果想在本地端測試是否可以成功跑專案，請使用cmd並輸入:<br>
```js     
yarn build
yarn start
```
打開http://localhost:8080/，<br>
可以看到利用express成功跑起專案(因為server.js設定8080，所以port為8080)<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/4.png" width="60%" height="60%"><br><br>
接著新增.env檔案，<br>
內容為GENERATE_SOURCEMAP=false，<br>
這個步驟是避免在部署的時候原始碼流出，<br>
Source maps允許你的原始碼在生產環境上，<br>
這使得debug上更加方便。<br>
然而這意味著任何人都可以看到妳的原始碼，所以新增這個檔案是避免有心人士看到程式碼。<br>
最後新增完資料夾的內容，可以看到多了server.js、.env，更改package.json檔案。<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/5.png" width="30%" height="40%"><br><br>

## 2.創建一個heroku應用程式 <br>
`為什麼要使用heroku?`<br>
Heroku為支援多種程式語言的雲端平台，<br>
一開始只限於支援Ruby，<br>
後來增加多種語言像是Java、Node.js、Scala、Clojure、Python以及PHP和Perl的支援。<br>
react app也可以掛在github page，<br>
然而github page只能掛載靜態網頁，<br>
若是想要結合後端運用的動態網頁，<br>
可能就得選用heroku，<br>
不過heroku一個月僅提供450小時，<br>
且30分鐘沒使用主機會進入休眠。<br><br>

先創建一個heroku帳號，有了帳號新增一個應用程式<br>
按下new<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/6.png" width="60%" height="60%"><br><br>
選擇create new app<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/7.png" width="60%" height="60%"><br><br>
輸入你想要的應用程式名稱<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/8.png" width="60%" height="60%"><br><br>
成功創建應用程式的畫面<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/9.png" width="60%" height="60%"><br><br>

## 3.將創建的app部署至heroku <br>
請先安裝Install the Heroku CLI，其為管理、創建、提交等命令的工具。<br>
https://devcenter.heroku.com/articles/heroku-cli<br>
一樣打開cmd<br>
登入heroku帳號<br>
在cmd打<br>
```js     
heroku login
```
跳出登入畫面，按一下login即可登入<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/10.png" width="30%" height="40%"><br><br>
成功登入畫面<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/11.png" width="30%" height="40%"><br><br>
接下來Create a new Git repository<br>
Initialize a git repository in a new or existing directory<br>
先進入到剛剛創建的react app資料夾<br>
```js     
$ cd my-project/
$ git init
$ heroku git:remote -a websocketreact
```
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/12.png" width="60%" height="60%"><br><br>
接下來可以部署至heroku<br>
```js     
$ git add .
$ git commit -m "make it better"
$ git push heroku master
```
Push上去heroku上<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/13.png" width="60%" height="60%"><br><br>
成功push畫面<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/14.png" width="60%" height="60%"><br><br>
若出現以下錯誤<br>
**Two different lockfiles found: package-lock.json and yarn.lock**
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/15.png" width="60%" height="60%"><br><br>
請輸入以下指令<br>
```js     
If you use npm:
git rm yarn.lock
git commit -m "Remove yarn lock file"
git push heroku master
If you use yarn:
git rm package-lock.json
git commit -m "Remove npm lock file"
```
git push heroku master<br>
接下來輸入heroku open，就可以看到成功上傳至heroku上了<br>
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/16.png" width="60%" height="60%"><br><br>

## 參考網站 <br>
先前有部署至heroku的專案:<br>
https://web-socket123.herokuapp.com/<br>
https://medium.com/jeremy-gottfrieds-tech-blog/tutorial-how-to-deploy-a-production-react-app-to-heroku-c4831dfcfa08<br>
https://github.com/facebook/create-react-app<br>
