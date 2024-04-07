<script>
import Swal from 'sweetalert2'
export default {
    data() {
        return {
            info: {
                text: "",
                amount: ""
            },
            infoList: [] // 新增一個變數用來儲存多筆資料
        }
    },
    props: [
        "active"
    ],
    emits: [
        "close", "add"
    ],
    methods: {
        closeInput() {
            this.$emit("close", false)
            // 清空输入框
            this.info.text = "";
            this.info.amount = "";

        },
        addInfo() {
            if (!this.info.text || !this.info.amount) {
                Swal.fire({
                    title: 'Please fill in the "Name" and "Amount" completely',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; // 如果有任何一項為空，停止執行
            }
            // 验证 amount 是否为正或负数
            if (!/^-?\d+$/.test(this.info.amount)) {
                // 使用 SweetAlert2 弹出警告框
                Swal.fire({
                    title: 'The first character can only be +- plus any amount of money.',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; // 如果不是正或负整数，停止执行
            }
            // 建立一個新資料
            const newInfo = {
                text: this.info.text,
                amount: this.info.amount
            };

            // 將新資料添加到陣列
            this.infoList.push(newInfo);
            // 清空输入框
            this.info.text = "";
            this.info.amount = "";
            // 觸發事件或执行其他邏輯
            this.$emit("add", newInfo);
        }
    }
}
</script>
<template>
    <div class="bgArea1" v-if="active">
        <div class="area1">
            <i class="fa-solid fa-xmark" @click="closeInput"></i>
            <!-- <button type="button" class="btn-close1" aria-label="Close" @click="closeInput">X</button> -->
            <div class="areaContent1">

                <label class="contentText1" for="">Name</label>
                <div class="blockText1">
                    <input class="contentInput1" type="text" v-model="info.text" placeholder="">
                </div>

                <label class="contentText1" for="">Amount</label>
                <div class="blockText1">
                    <input class="contentInput1" type="text" v-model="info.amount" placeholder=" +/- amount">
                </div>
            </div>
            <!-- 在子層面板顯示輸入資料 -->
            <!-- <p>{{ info }}</p>   -->
            <button class="btnAdd1" type="button" @click="addInfo"><i class="fa-solid fa-plus"></i>Add
                transaction</button>
            <!--  在子層面板顯示多筆輸入資料 -->
            <!-- <div class="info-list"> 
                <div v-for="(item, index) in infoList" :key="index"> 
                    <p>Text: {{ item.text }}</p>
                    <p>Amount: {{ item.amount }}</p> 
                </div> 
            </div>  -->
        </div>
    </div>
</template>
<style lang="scss" scoped>
$maincolor: #2e4f9b;

.bgArea1 {
    width: 100vw;
    height: 100vh;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    .area1 {
        width: 400px;
        height: 400px;
        background-color: white;
        border-radius: 30px;
        display: flex;
        flex-direction: column;
        align-items: center;
        padding: 20px 0;
        position: relative;

        .fa-xmark {
            font-size: 24pt;
            color: $maincolor;
            margin-left: 320px;

            &:hover {
                cursor: pointer;
                color: #7a92ca;
            }
        }

        .areaContent1 {

            .contentText1 {
                margin-top: 20px;
                margin-bottom: 5px;
                color: $maincolor;

            }

            .contentInput1 {
                width: 250px;
                height: 35px;
                border: 2px solid $maincolor;
                border-radius: 6px;
                padding-left: 5px;
            }
        }

        .btnAdd1 {
            width: 150px;
            height: 35px;
            background-color: $maincolor;
            color: white;
            border-radius: 10px;
            margin-top: 60px;

            &:hover {
                background-color: #8e9ba5;
            }
        }
    }
}
</style>