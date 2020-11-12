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
                <v-card-title>{{ questionTitle }}</v-card-title>
                <v-card-text>
                    <div>
                        {{ questionDesc }}
                    </div>
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
                        color="white"
                        class="teal accent-4 font-weight-bold"
                        text
                        @click="addCurrentNum()"
                    >
                        スタート
                    </v-btn>
                </v-card-actions>
            </v-card>

            <!-- クイズページ -->
            <v-card
            class="quiz-container"
            v-else-if="currentNum > 0 && currentNum <= questionList.length"
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
                    v-for="option in question.options"
                    :key="option.id"
                    >
                        <input
                        type="radio"
                        :id="option.uniqueId"
                        :value="option"
                        v-model="checkedItem"
                        @click="checkState()"
                        >
                        <label
                        :for="option.uniqueId"
                        :class="{ 'isSelected': checkedItem === option, 'isChecked': isChecked }"
                        >
                            {{ option.option }}
                        </label>
                    </div>

                </v-card-text>
                </div>

                <v-card-actions>
                    <!-- 「次へ」ボタン -->
                    <v-btn
                    color="white"
                    class="light-blue accent-4 font-weight-bold"
                    text
                    @click="getNextQuiz()"
                    :disabled="!isChecked"
                    v-if="currentNum > 0 && currentNum <= (questionList.length - 1)"
                    >
                        次へ
                    </v-btn>
                    <!-- 「結果」ボタン -->
                    <v-btn
                    color="white"
                    class="cyan accent-4 font-weight-bold"
                    text
                    @click="getResultQuiz()"
                    v-else-if="currentNum >= questionList.length"
                    >
                        結果
                    </v-btn>
                </v-card-actions>
            </v-card>

            <!-- 結果ページ -->
            <v-card
            class="quiz-container"
            v-else-if="currentNum > questionList.length"
            >
                <v-card-title>結果ページ</v-card-title>
                <v-card-text>
                    <div class="mb-2">
                        <span v-if="userName !== null">{{ userName }}さん</span>
                        <span v-else>{{ defaultName }}さん</span>
                        は
                        <span>{{ totalPoints }}</span>
                        点でした
                    </div>
                    <div class="selected">
                        <p>あなたの回答</p>
                        <ul class="selected__list">
                            <li
                            v-for="(item, index) in selectedItem"
                            class="selected__item"
                            :key="item.uniqueId"
                            >
                            問題{{ index + 1 }}: {{ item.option }}
                            <span
                            v-if="item.isCorrect"
                            class="font-weight-bold"
                            >
                            正解！
                            </span>
                            </li>
                        </ul>
                    </div>
                </v-card-text>

                <v-card-actions>
                    <!-- 「リセット」ボタン -->
                    <v-btn
                    color="white"
                    class="green darken-1 font-weight-bold"
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
export default {
    data() {
        return {
            currentNum: 0, // 何番目の問題か判断するデータ
            questionTitle: '三択クイズ', // クイズのタイトル
            questionDesc: '選択肢から回答を選んで次へを押す', // クイズの説明文
            userName: null, // 入力されたプレイヤーの名前を格納
            defaultName: '名無し', // 名前が入力されなかったときのデフォルト名
            isChecked: false, // チェックの有無
            totalPoints: 0, // 正解数
            checkedItem: [], // 選択した選択肢を格納するデータ
            selectedItem: [], // 選択した選択肢一覧を格納するデータ
            fixedSelectItems: [], // 選択した選択肢のうちを正解のみを格納する配列
            imgSrc: 'question-img', // 画像ファイルの共通パス
            questionList: [ // 問題情報
                {
                    id: 1, // 問題番号
                    question: '問題1', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: '選択肢1', // 選択肢
                            isCorrect: true, // 選択肢の正誤
                            uniqueId: 1
                        },
                        {
                            id: 2, // 選択肢番号
                            option: '選択肢2', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 2
                        },
                        {
                            id: 3, // 選択肢番号
                            option: '選択肢3', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 3
                        },
                    ]
                },
                {
                    id: 2, // 問題番号
                    question: '問題2', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: '選択肢1', // 選択肢
                            isCorrect: true, // 選択肢の正誤
                            uniqueId: 4
                        },
                        {
                            id: 2, // 選択肢番号
                            option: '選択肢2', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 5
                        },
                        {
                            id: 3, // 選択肢番号
                            option: '選択肢3', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 6
                        },
                    ]
                },
                {
                    id: 3, // 問題番号
                    question: '問題3', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: '選択肢1', // 選択肢
                            isCorrect: true, // 選択肢の正誤
                            uniqueId: 7
                        },
                        {
                            id: 2, // 選択肢番号
                            option: '選択肢2', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 8
                        },
                        {
                            id: 3, // 選択肢番号
                            option: '選択肢3', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 9
                        },
                    ]
                },
                {
                    id: 4, // 問題番号
                    question: '問題4', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: '選択肢1', // 選択肢
                            isCorrect: true, // 選択肢の正誤
                            uniqueId: 10
                        },
                        {
                            id: 2, // 選択肢番号
                            option: '選択肢2', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 11
                        },
                        {
                            id: 3, // 選択肢番号
                            option: '選択肢3', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 12
                        },
                    ]
                },
                {
                    id: 5, // 問題番号
                    question: '問題5', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: '選択肢1', // 選択肢
                            isCorrect: true, // 選択肢の正誤
                            uniqueId: 13
                        },
                        {
                            id: 2, // 選択肢番号
                            option: '選択肢2', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 14
                        },
                        {
                            id: 3, // 選択肢番号
                            option: '選択肢3', // 選択肢
                            isCorrect: false, // 選択肢の正誤
                            uniqueId: 15
                        },
                    ]
                },
            ]
        }
    },
    methods: {
        // スタートボタンの処理
        addCurrentNum: function() {
            this.currentNum ++
        },
        // 問題を進める処理
        getNextQuiz: function() {
            this.addCurrentNum()
            this.isChecked = false

            this.selectedItem.push(this.checkedItem)

            if (this.checkedItem.isCorrect) {
                this.fixedSelectItems.push(this.checkedItem)
            }
        },
        // 結果を出す処理
        getResultQuiz: function() {
            this.getNextQuiz()

            this.totalPoints = this.fixedSelectItems.length
        },
        // リセット処理
        getResetQuiz: function() {
            this.currentNum = 0
            this.userName = null
            this.isChecked = false
            this.totalPoints = 0
            this.checkedItem = []
            this.selectedItem = []
            this.fixedSelectItems = []
        },
        // 選択肢をチェックしてるかどうかの状態を取得
        checkState: function() {
            this.isChecked = true
        }
    }
}
</script>

<style lang="scss">
    .v-btn {
        &--disabled {
            opacity: .6;
        }
    }

    .v-application {
        p {
            margin-bottom: 0 !important;
        }
    }

    .quiz {
        max-width: 400px;
        margin: 50px auto 0;
        width: 100%;

        &-question {
            display: none;

            &.active {
                display: block;
            }
        }

        .isChecked {
            opacity: .4;
        }

        .isSelected {
            font-weight: bold;
            opacity: 1;
        }
    }

    .selected {
        &__list {
            list-style: none;
            padding-left: 0 !important;
        }
    }
</style>