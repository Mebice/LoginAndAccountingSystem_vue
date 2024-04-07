<script>
import Swal from 'sweetalert2'
import InputAcc from "../components/InputAcc.vue";
import DeleteAcc from "../components/DeleteAcc.vue";
export default {
    data() {
        return {
            storedEmail: '', // 用來存儲email

            // 預設變數為false
            inputActive: false, //控制是否彈出輸入框。
            infoList: [],  //  存儲多筆資料的陣列
            income: 0,   //  INCOME收入 初始為0
            expense: 0,   //  EXPENSE支出 初始為0
            deleteActive: false,  //控制是否彈出刪除框
            deleteIndex: -1, // 初始化 deleteIndex 為 -1，表示未選定要刪除的項目
        }
    },
    components: {
        InputAcc,
        DeleteAcc
    },
    created() {
        // 在組件創建時從 sessionStorage 中獲取 email
        this.storedEmail = sessionStorage.getItem('email');
    },
    methods: {
        // 彈出輸入框方法為點擊後變true
        openInput() {
            this.inputActive = true;
        },
        //關閉輸入框
        inputCancel(n) {
            this.inputActive = n;
        },
        //接收新的資料並添加到 infoList 陣列中，同時更新收入和支出總額
        infoShow(newInfo) {
            const info = {
                text: newInfo.text,
                amount: newInfo.amount,
                infoButton: true, // 添加来顯示按钮
            }
            this.infoList.push(info);

            // 根據金額正負來更新 INCOME 或 EXPENSE
            if (newInfo.amount > 0) {
                this.income += parseFloat(newInfo.amount);
            } else {
                this.expense -= parseFloat(newInfo.amount);
            }
        },
        //彈出刪除框
        openDelete(index) {
            this.deleteActive = true;
            this.deleteIndex = index; // 設定要刪除的項目的索引
        },
        //關閉刪除框
        deleteCancel() {
            this.deleteActive = false;
        },
        reDelete() {
            // 在這個方法中執行刪除和金額更新的操作
            // 1. 刪除清單中的項目
            // 2. 更新金額 (income 和 expense)
            if (this.deleteIndex >= 0 && this.deleteIndex < this.infoList.length) {
                // 找到要刪除的項目
                const deletedItem = this.infoList[this.deleteIndex];

                // 更新金額
                if (deletedItem.amount > 0) {
                    this.income -= deletedItem.amount;
                } else {
                    this.expense -= Math.abs(deletedItem.amount);
                }

                // 從 infoList 中刪除項目
                this.infoList.splice(this.deleteIndex, 1);
            }

            // 重置 deleteIndex 變數
            this.deleteIndex = -1;
        },
        navigateToLogin() {
            Swal.fire({
                title: 'sign out?',
                icon: 'question',
                showCancelButton: true,
                confirmButtonColor: '#d33',
                cancelButtonColor: '#3085d6',
                confirmButtonText: 'Yes',
                cancelButtonText: 'No'
            }).then((result) => {
                //點擊確認按鈕後
                if (result.isConfirmed) {
                    this.$router.push('/'); // 導航到登入頁面
                }
            })
        }
    },


    computed: {
        //計算屬性
        balance() {
            return this.income - this.expense;  //計算收入減去支出以計算總餘額
        },
    },
}
</script>

<template>
    <div class="bgArea">
        <div class="blueArea">
            <h1 class="blueAreaText">Expense Tracker</h1>
            <h1 class="blueAreaText2"> User : {{ storedEmail }}</h1>
            <h3 class="blueAreaText2">YOUR BALANCE</h3>
            <h1 class="blueAreaText2">${{ balance }}</h1>
            <button class="LoginBtn" @click="navigateToLogin"><i class="fa-solid fa-power-off"></i>
                Sign out</button>

        </div>
        <!-- 白色區域 -->
        <div class="whiteArea">
            <div class="top">
                <div class="topLeft">
                    <h1>INCOME</h1>
                    <h1>${{ income }}</h1>
                </div>
                <div class="topRight">
                    <h1>EXPENSE</h1>
                    <h1>${{ expense }}</h1>
                </div>
            </div>
            <!-- 點擊按鈕彈出輸入視窗:方法 -->
            <button type="button" class="numBtn" @click="openInput"><i class="fa-solid fa-plus"></i>
                Add transaction
            </button>
            <div class="content">
                <div class="contentList" v-for="(item, index) in infoList" :key="index">

                    <p class="listText">{{ item.text }}</p>
                    <div class="listArea">
                        <p :class="{ 'positive': item.amount > 0, 'negative': item.amount < 0 }">${{ item.amount }}</p>
                        <!-- 刪除按鈕 -->
                        <button class="listBtn" @click="openDelete(index)"> Delete </button>
                    </div>
                </div>
            </div>
        </div>
        <!-- 匯入的輸入框 -->
        <div class="inputArea">
            <div class="inputInner">
                <InputAcc :active="inputActive" @close="inputCancel()" @add="infoShow" />
            </div>
        </div>
        <div class="deleteInner">
            <DeleteAcc :goDelete="deleteActive" @close2="deleteCancel()" @close3="deleteCancel()"
                @deleteConfirmed="reDelete" />
        </div>
    </div>
</template>

<style lang="scss" scoped>
$maincolor: #324b76;

.bgArea {
    width: 100vw;
    display: flex;

    .blueArea {
        width: 50%;
        background-color: $maincolor;
        display: flex;
        flex-direction: column;
        align-items: center;
        position: relative;
        padding: 134px 0;

        .blueAreaText {
            color: white;
        }

        .blueAreaText2 {
            color: white;
            margin-top: 40px;
        }

        .LoginBtn {
            width: 120px;
            height: 40px;
            padding: 6px 10px;
            background-color: #fff;
            border-radius: 10px;
            position: absolute;
            bottom: 30px;
            left: 30px;

            &:hover {
                background-color: #a3a7b8;
            }
        }

    }

    .whiteArea {
        width: 50%;
        display: flex;
        flex-direction: column;
        align-items: center;

        .top {
            width: 100%;
            display: flex;
            flex-wrap: wrap;
            align-items: center;
            justify-content: space-between;
            margin-top: 60px;
            // background-color: #c4ab21;
            padding: 0 80px;
        }

        .topLeft {
            color: rgb(65, 189, 65);
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .topRight {
            color: rgb(190, 27, 27);
            display: flex;
            flex-direction: column;
            align-items: center;
        }
    }

    .numBtn {
        padding: 3px 8px;
        border-radius: 10px;
        color: white;
        background-color: $maincolor;
        font-size: 16pt;
        margin-top: 20px;

        &:hover {
            background-color: #676b71;
        }
    }

    // 記帳列表
    .content {
        margin-top: 20px;
        overflow-y: overlay;
        max-block-size: 450px;

        .contentList {
            position: relative;
            width: 400px;
            height: 40px;
            border: 3px solid $maincolor;
            border-radius: 6px;
            display: flex;
            margin-bottom: 12px;

            .listText {
                font-size: 18pt;
                position: absolute;
                left: 10px;
                top: -3px;
            }

            .positive {
                color: rgb(36, 149, 36);
                font-size: 20pt;
                position: absolute;
                right: 90px;
                top: -3px;
            }

            .negative {
                color: rgb(209, 19, 19);
                font-size: 20pt;
                position: absolute;
                right: 90px;
                top: -3px;
            }

            .listBtn {
                position: absolute;
                right: 2px;
                top: 2px;
                color: white;
                background-color: $maincolor;
                border-radius: 6px;
            }
        }
    }
}

.inputArea {
    position: fixed;
    top: 0;
    left: 0px;
}

.deleteInner {
    position: fixed;
    top: 0;
    left: 0px;
}
</style>