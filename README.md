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

