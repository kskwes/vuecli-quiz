# vuecli-quiz

## vue-cliでクイズアプリを作ってみる
 
**ローカルサーバーを立ち上げる**
 
`npm run serve`

**vue.config.jsに以下を記述してからbuild**
 
```
module.exports = {
    publicPath: '/vuecli-quiz/',
    outputDir: 'docs'
  }
```
 
`npm run build`

## 仕様
- 合計5問の3択問題
- 最後に合計正解数を表示
- 問題は```questionList```に格納
## 使ったもの
- Vue-Cli
- Vuetify