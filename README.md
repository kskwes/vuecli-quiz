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
### demo
https://kskwes.github.io/vuecli-quiz/

- 合計5問の3択問題
- 最後に合計正解数を表示
- 問題は```questionList```に格納
```
questionList: [ // 問題情報
    {
        id: 1, // 問題番号
        question: '問題1', // 問題文
        options: [ // 選択肢情報
            {
                id: 1, // 選択肢番号
                option: '選択肢1', // 選択肢
                isCorrect: true, // 選択肢の正誤
                uniqueId: 1 // 何問目何番目の選択肢かどうかの識別番号
            },
        ]
    },
]
```

## 使ったツール
- Vue-Cli
- Vuetify