<template>
    <v-app>
        <div class="quiz">
            <!-- スタートページ -->
            <v-card
            elevation="2"
            tile
            class="quiz-container"
            v-if="currentNum <= 0"
            >
                <v-card-title>三択クイズ</v-card-title>
                <v-card-text>
                    <div>
                        <v-text-field
                        v-model="userName"
                        placeholder="名前を入力してね"
                        ></v-text-field>
                    </div>
                </v-card-text>

                <v-card-actions>
                    <!-- 「スタート」ボタン -->
                    <v-btn
                        color="lighten-2"
                        text
                        @click="getStartQuiz()"
                    >
                        スタート
                    </v-btn>
                </v-card-actions>
            </v-card>

            <!-- クイズページ -->
            <v-card
            class="quiz-container"
            v-else-if="currentNum > 0 && currentNum <= questionNum"
            >
                <div
                v-for="question in questionList"
                :key="question.id"
                :class='{ "active": currentNum === question.id }'
                class="quiz-question"
                >
                <v-card-title>{{ question.question }}</v-card-title>
                <v-card-text>
                    <div
                    v-for="(option, index) in question.options"
                    :key="option.id"
                    >
                        <input
                        type="radio"
                        :id="option.option"
                        :value="option"
                        v-model="checkedItem"
                        @click="checkState()"
                        :class="{ 'isSelected': checkedItem === option.option }"
                        >
                        <label
                        :for="option.option"
                        >
                            {{ index + 1 }}, {{ option.option }}
                        </label>
                    </div>
                </v-card-text>
                </div>

                <v-card-actions>
                    <!-- 「次へ」ボタン -->
                    <v-btn
                    color="lighten-2"
                    text
                    @click="getNextQuiz()"
                    :disabled="!isChecked"
                    v-if="currentNum > 0 && currentNum <= (questionNum - 1)"
                    >
                        次へ
                    </v-btn>
                    <!-- 「結果」ボタン -->
                    <v-btn
                    color="lighten-2"
                    text
                    @click="getResultQuiz()"
                    v-else-if="currentNum >= questionNum"
                    >
                        結果
                    </v-btn>
                </v-card-actions>
            </v-card>

            <!-- 結果ページ -->
            <v-card
            class="quiz-container"
            v-else-if="currentNum > questionNum"
            >
                <v-card-title>結果ページ</v-card-title>
                <v-card-text>
                    <div>
                        <span v-if="userName !== null">{{ userName }}さん</span>
                        <span v-else>名無しさん</span>
                        は
                        <span>{{ totalPoints }}</span>
                        点でした
                    </div>
                    <div>
                        <p>あなたの回答</p>
                        <ul>
                            <li
                            v-for="item in selectedItem"
                            :key="item.option"
                            >
                            {{ item.option }}
                            <span v-if="item.isCorrect">正解！</span>
                            </li>
                        </ul>
                    </div>
                </v-card-text>

                <v-card-actions>
                    <!-- 「リセット」ボタン -->
                    <v-btn
                    color="lighten-2"
                    text
                    @click="getResetQuiz()"
                    >
                        もう一度
                    </v-btn>
                </v-card-actions>
            </v-card>
        </div>
    </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
    data() {
        return {
            currentNum: 0, // 何番目の問題か判断するデータ
            questionNum: 5, // 問題数を設定するデータ
            optionsNum: 3, // 選択肢数を設定するデータ
            questionDesc: '', // クイズの説明文
            userName: null, // 入力されたプレイヤーの名前を格納
            isChecked: false, // チェックの有無
            isSelected: false, // チェックの有無
            totalPoints: 0, // 正解数
            checkedItem: [], // 選択した選択肢を格納するデータ
            selectedItem: [], // 選択した選択肢一覧を格納するデータ
            fixedSelectItems: [], // 選択した選択肢のうちを正解のみを格納する配列
            questionList: [ // 問題情報
                {
                    id: 1, // 問題番号
                    question: 'あああ?', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: 'あああ', // 選択肢
                            isCorrect: true// 選択肢の正誤
                        },
                        {
                            id: 2, // 選択肢番号
                            option: 'いいい', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                        {
                            id: 3, // 選択肢番号
                            option: 'ううう', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                    ]
                },
                {
                    id: 2, // 問題番号
                    question: 'えええ?', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: 'えええ', // 選択肢
                            isCorrect: true// 選択肢の正誤
                        },
                        {
                            id: 2, // 選択肢番号
                            option: 'おおお', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                        {
                            id: 3, // 選択肢番号
                            option: 'かかか', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                    ]
                },
                {
                    id: 3, // 問題番号
                    question: 'ききき?', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: 'ききき', // 選択肢
                            isCorrect: true// 選択肢の正誤
                        },
                        {
                            id: 2, // 選択肢番号
                            option: 'くくく', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                        {
                            id: 3, // 選択肢番号
                            option: 'けけけ', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                    ]
                },
                {
                    id: 4, // 問題番号
                    question: 'こここ?', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: 'こここ', // 選択肢
                            isCorrect: true// 選択肢の正誤
                        },
                        {
                            id: 2, // 選択肢番号
                            option: 'さささ', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                        {
                            id: 3, // 選択肢番号
                            option: 'ししし', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                    ]
                },
                {
                    id: 5, // 問題番号
                    question: 'すすす?', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: 'すすす', // 選択肢
                            isCorrect: true// 選択肢の正誤
                        },
                        {
                            id: 2, // 選択肢番号
                            option: 'せせせ', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                        {
                            id: 3, // 選択肢番号
                            option: 'そそそ', // 選択肢
                            isCorrect: false// 選択肢の正誤
                        },
                    ]
                },
            ]
        }
    },
    methods: {
        // スタートボタンの処理
        getStartQuiz: function() {
            this.currentNum ++
        },
        // 問題を進める処理
        getNextQuiz: function() {
            this.currentNum ++
            this.isChecked = false

            this.selectedItem.push(this.checkedItem)

            if (this.checkedItem.isCorrect) {
                this.fixedSelectItems.push(this.checkedItem)
            }
        },
        // 結果を出す処理
        getResultQuiz: function() {
            this.currentNum ++
            this.isChecked = false

            this.selectedItem.push(this.checkedItem)

            if (this.checkedItem.isCorrect) {
                this.fixedSelectItems.push(this.checkedItem)
            }

            this.totalPoints = this.fixedSelectItems.length
        },
        // リセット処理
        getResetQuiz: function() {
            this.currentNum = 0
            this.userName = null
            this.isChecked = false
            this.isSelected = false
            this.totalPoints = 0
            this.checkedItem = []
            this.selectedItem = []
            this.fixedSelectItems = []
        },
        // 回答チェック
        checkState: function() {
            this.isChecked = true
        }
    }
}
</script>

<style lang="scss">
    .quiz {
        max-width: 500px;
        margin: 50px auto 0;
        width: 100%;

        &-container {
            
        }

        &-question {
            display: none;

            &.active {
                display: block;
            }
        }
    }
</style>
