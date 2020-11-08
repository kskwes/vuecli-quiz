<template>
    <div class="quiz">
        <!-- クイズの問題をループで表示 -->
        <!-- ここの表示切り替えはVuetifyのCarouselで再現できるかも？ -->
        <!-- スタート画面を0とし、「スタート」「次へ」ボタンで表示を切り替える（数字を増やす） -->
        <!-- 数字が最後になったら、結果ページを表示 -->

        <!-- スタートページ -->
        <div class="quiz-start">
            <div class="quiz-start__description">
                <!-- 説明文 -->
            </div>
            <div class="quiz-start__username">
                <!-- プレイヤーの名前を入力して取得 -->
            </div>
            
            <!-- 「スタート」ボタン -->
            <div class="start">

            </div>
        </div>

        <!-- クイズページ -->
        <div class="quiz-question">
            <!-- 問題部分もVuetifyでできるかも（<v-card></v-card>） -->
            <!-- クイズの問題文 -->
            <div class="quiz-question__title">

            </div>
            <!-- クイズの選択肢 -->
            <ul class="quiz-options">
                <!-- ループで選択肢を表示 -->
                <li class="quiz-options__option">
                    <input type="checkbox" id="" value="" v-model="checkedOption">
                    <label for=""></label>
                </li>
            </ul>

            <div class="btn">
                <!-- 「次へ」ボタン -->
                <div class="next">

                </div>
                <!-- 「結果」ボタン -->
                <div class="result">

                </div>
            </div>
        </div>

        <!-- 結果ページ -->
        <div class="quiz-result">
            <div class="quiz-result__comment">
                <!-- 正解数 -->
            </div>
            <!-- 選択した選択肢をループで表示 -->
            <ul class="selected-list">
                <li class="selected-list__item">

                </li>
            </ul>

            <div class="btn">
                <!-- 「リセット」ボタン -->
                <div class="reset">
                    <!-- データをすべてリセットして最初からプレイ -->
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            currentNum: 0, // 何番目の問題か判断するデータ
            questionNum: 5, // 問題数を設定するデータ
            optionsNum: 3, // 選択肢数を設定するデータ
            questionDesc: '', // クイズの説明文
            userName: '', // 入力されたプレイヤーの名前を格納
            selectedItems: [], // 選択肢した選択肢を格納する配列
            questionList: [ // 問題情報
                {
                    id: 1, // 問題番号
                    question: '', // 問題文
                    options: [ // 選択肢情報
                        {
                            id: 1, // 選択肢番号
                            option: '', // 選択肢
                            isCorrect: true// 選択肢の正誤
                        },
                    ]
                },
            ]
        }
    },
    methods: {
        // スタートボタンの処理
        getStartQuiz: function() {
            // currentNumを1増やす
        },
    }
}
</script>