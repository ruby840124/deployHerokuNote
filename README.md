# deployHerokuNote
之前有將React app部署到heroku，<br>
故整理一下筆記~ <br>
總共有兩個步驟:<br>
**第一步驟是創建一個react app** <br>
**第二步驟是將創建的app部署至heroku** <br>

## 創建一個react app <br>
`為什麼要使用react app?`<br>
React app為臉書開發的快速建置專案環境的工具。<br>
在自己建置react專案時，<br>
需要babel轉譯語法，<br>
且還需要用webpack打包程式碼，<br>
有了react app可以輸入幾行指令立刻幫你打包這些react所需的環境。<br>

**1.創建一個react app專案** <br>
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
<img src="https://github.com/ruby840124/deployHerokuNote/blob/main/herokuImg/5.png" width="60%" height="60%"><br><br>



