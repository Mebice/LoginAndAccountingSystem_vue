<script>
import Swal from 'sweetalert2'
export default {
    data() {
        return {
            pwdVisible: true, //形式:隱藏密碼
            pwdVisible2: true, //形式:隱藏密碼
            email: '',
            password: '',
            repassword: '',
        }
    },
    methods: {
        changePwd() {
            this.pwdVisible = !this.pwdVisible;
        },
        changePwd2() {
            this.pwdVisible2 = !this.pwdVisible2;
        },

        register() { //註冊
            // 檢查email和password是否為空
            if (!this.email || !this.password || !this.repassword) {
                Swal.fire({
                    title: 'Please enter your email and password and repeated password.',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; // 如果email或password有任一為空，停止執行
            }

            //密碼和重複密碼必須相同
            if (this.password != this.repassword) {
                Swal.fire({
                    title: 'Password and repeat password must be the same.',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; //停止執行
            }

            // 從 session storage 中獲取儲存的用戶資訊
            const storedEmail = sessionStorage.getItem('email');
            const storedPassword = sessionStorage.getItem('password');

            // 比對輸入的 email 是否與 session storage 中的相符，相同顯示該email已存在
            if (storedEmail === this.email) {
                Swal.fire({
                    title: 'The email already exists.',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; // 如果email已存在，停止執行
            }

            // 密码格式验证：必须包含数字、字母，长度在6到16位，且不能包含空格
            const passwordRegex = /^(?=.*[A-Za-z])(?=.*\d)[A-Za-z\d]{6,16}$/;
            if (!passwordRegex.test(this.password)) {
                Swal.fire({
                    title: 'Password must contain at least one letter, one number, and be 6-16 characters long. Spaces are not allowed.',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; // 停止执行
            }

            // 验证邮箱格式是否正确
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailRegex.test(this.email)) {
                Swal.fire({
                    title: 'Please enter a valid email address.',
                    icon: 'warning',
                    confirmButtonColor: '#3085d6',
                    confirmButtonText: 'OK'
                });
                return; // 停止执行
            }

            //如果註冊成功
            // 儲存 email 和 password 至 sessionStorage
            sessionStorage.setItem('email', this.email);
            sessionStorage.setItem('password', this.password);

            Swal.fire({
                icon: 'success',
                confirmButtonColor: '#3085d6',
                confirmButtonText: 'OK'
            }).then(() => {
                this.$router.push('/'); // 導航至登入頁面
            });
        },
        navigateToLogin() {
            this.$router.push('/'); //取消 導航回登入頁面的根路徑
        },
    },

}
</script>
<template>
    <!-- 大區域 -->
    <div class="areaSign">
        <!-- 標題文字 -->
        <div class="pageSign">
            <h1>Sign up</h1>
        </div>
        <!-- 包住輸入區域控制位置，因為用整個背景margin推和用position相對位置會跑版 -->
        <div class="inputArea">
            <!-- Account文字 -->
            <div class="textSign">
                <h6>Email</h6>
            </div>
            <!-- Account輸入格 -->
            <div class="blockSign">
                <i class="fa-solid fa-envelope"></i>
                <input type="text" class="inputSign" v-model="email">
            </div>
            <!-- Password文字 -->
            <div class="textSign">
                <h6>Password</h6>
            </div>
            <!-- Password輸入格 -->
            <div class="blockSign">
                <i class="fa-solid fa-lock lock1"></i>
                <input class="inputSign" :type="pwdVisible ? 'password' : 'text'" v-model="password">
                <svg class="closeEye" v-if="pwdVisible" @click="changePwd" data-v-1932284b="" width="20" height="20"
                    viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path data-v-1932284b="" fill-rule="evenodd" clip-rule="evenodd"
                        d="M17.5753 6.85456C17.7122 6.71896 17.8939 6.63806 18.0866 6.63806C18.7321 6.63806 19.0436 7.42626 18.5748 7.87006C18.1144 8.30554 17.457 8.69885 16.6478 9.03168L18.1457 10.5296C18.2101 10.5941 18.2613 10.6706 18.2962 10.7548C18.331 10.839 18.349 10.9293 18.349 11.0204C18.349 11.1116 18.331 11.2019 18.2962 11.2861C18.2613 11.3703 18.2101 11.4468 18.1457 11.5113C18.0812 11.5757 18.0047 11.6269 17.9205 11.6618C17.8363 11.6967 17.746 11.7146 17.6548 11.7146C17.5637 11.7146 17.4734 11.6967 17.3892 11.6618C17.305 11.6269 17.2284 11.5757 17.164 11.5113L15.3409 9.68819C15.2898 9.63708 15.247 9.57838 15.2141 9.51428C14.4874 9.71293 13.6876 9.87122 12.8344 9.98119C12.8363 9.99011 12.8381 9.99908 12.8397 10.0081L13.2874 12.5472C13.315 12.7266 13.2713 12.9098 13.1656 13.0573C13.0598 13.2049 12.9005 13.3052 12.7217 13.3367C12.5429 13.3683 12.3589 13.3285 12.2091 13.2259C12.0592 13.1234 11.9555 12.9663 11.9202 12.7882L11.4725 10.2491C11.4645 10.2039 11.4611 10.1581 11.4621 10.1125C10.9858 10.1428 10.4976 10.1586 10.0002 10.1586C9.57059 10.1586 9.14778 10.1468 8.73362 10.1241C8.73477 10.1656 8.7322 10.2074 8.72578 10.249L8.27808 12.7881C8.24612 12.9694 8.14345 13.1306 7.99265 13.2362C7.84186 13.3418 7.65528 13.3831 7.47398 13.3512C7.29268 13.3192 7.1315 13.2166 7.0259 13.0658C6.9203 12.915 6.87892 12.7284 6.91088 12.5471L7.35858 10.008C7.35877 10.007 7.35896 10.0061 7.35915 10.0052C6.50085 9.90284 5.6941 9.75191 4.95838 9.56025C4.93012 9.60634 4.89634 9.64933 4.85748 9.68819L3.03438 11.5113C2.96992 11.5757 2.8934 11.6269 2.80918 11.6618C2.72496 11.6967 2.63469 11.7146 2.54353 11.7146C2.45237 11.7146 2.36211 11.6967 2.27789 11.6618C2.19367 11.6269 2.11714 11.5757 2.05268 11.5113C1.98822 11.4468 1.93709 11.3703 1.90221 11.2861C1.86732 11.2019 1.84937 11.1116 1.84937 11.0204C1.84937 10.9293 1.86732 10.839 1.90221 10.7548C1.93709 10.6706 1.98822 10.5941 2.05268 10.5296L3.49373 9.08855C2.6197 8.744 1.91247 8.33062 1.42559 7.87006C0.956591 7.42636 1.26799 6.63816 1.91359 6.63816C2.10629 6.63816 2.28789 6.71896 2.42489 6.85456C2.70009 7.12696 3.19529 7.45886 3.98459 7.77796C5.54429 8.40856 7.73699 8.77016 10.0001 8.77016C12.2632 8.77016 14.4558 8.40856 16.0156 7.77796C16.8049 7.45886 17.3001 7.12696 17.5753 6.85456Z"
                        fill="#fff"></path>
                </svg>
                <i v-else @click="changePwd" class="fa-solid fa-eye"></i>

            </div>
            <!-- Repeat Password文字 -->
            <div class="textSign">
                <h6>Repeat Password</h6>
            </div>
            <!-- Repeat Password輸入格 -->
            <div class="blockSign">
                <i class="fa-solid fa-lock lock2"></i>
                <input class="inputSign" :type="pwdVisible2 ? 'password' : 'text'" v-model="repassword">
                <svg class="closeEye2" v-if="pwdVisible2" @click="changePwd2" data-v-1932284b="" width="20" height="20"
                    viewBox="0 0 20 20" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path data-v-1932284b="" fill-rule="evenodd" clip-rule="evenodd"
                        d="M17.5753 6.85456C17.7122 6.71896 17.8939 6.63806 18.0866 6.63806C18.7321 6.63806 19.0436 7.42626 18.5748 7.87006C18.1144 8.30554 17.457 8.69885 16.6478 9.03168L18.1457 10.5296C18.2101 10.5941 18.2613 10.6706 18.2962 10.7548C18.331 10.839 18.349 10.9293 18.349 11.0204C18.349 11.1116 18.331 11.2019 18.2962 11.2861C18.2613 11.3703 18.2101 11.4468 18.1457 11.5113C18.0812 11.5757 18.0047 11.6269 17.9205 11.6618C17.8363 11.6967 17.746 11.7146 17.6548 11.7146C17.5637 11.7146 17.4734 11.6967 17.3892 11.6618C17.305 11.6269 17.2284 11.5757 17.164 11.5113L15.3409 9.68819C15.2898 9.63708 15.247 9.57838 15.2141 9.51428C14.4874 9.71293 13.6876 9.87122 12.8344 9.98119C12.8363 9.99011 12.8381 9.99908 12.8397 10.0081L13.2874 12.5472C13.315 12.7266 13.2713 12.9098 13.1656 13.0573C13.0598 13.2049 12.9005 13.3052 12.7217 13.3367C12.5429 13.3683 12.3589 13.3285 12.2091 13.2259C12.0592 13.1234 11.9555 12.9663 11.9202 12.7882L11.4725 10.2491C11.4645 10.2039 11.4611 10.1581 11.4621 10.1125C10.9858 10.1428 10.4976 10.1586 10.0002 10.1586C9.57059 10.1586 9.14778 10.1468 8.73362 10.1241C8.73477 10.1656 8.7322 10.2074 8.72578 10.249L8.27808 12.7881C8.24612 12.9694 8.14345 13.1306 7.99265 13.2362C7.84186 13.3418 7.65528 13.3831 7.47398 13.3512C7.29268 13.3192 7.1315 13.2166 7.0259 13.0658C6.9203 12.915 6.87892 12.7284 6.91088 12.5471L7.35858 10.008C7.35877 10.007 7.35896 10.0061 7.35915 10.0052C6.50085 9.90284 5.6941 9.75191 4.95838 9.56025C4.93012 9.60634 4.89634 9.64933 4.85748 9.68819L3.03438 11.5113C2.96992 11.5757 2.8934 11.6269 2.80918 11.6618C2.72496 11.6967 2.63469 11.7146 2.54353 11.7146C2.45237 11.7146 2.36211 11.6967 2.27789 11.6618C2.19367 11.6269 2.11714 11.5757 2.05268 11.5113C1.98822 11.4468 1.93709 11.3703 1.90221 11.2861C1.86732 11.2019 1.84937 11.1116 1.84937 11.0204C1.84937 10.9293 1.86732 10.839 1.90221 10.7548C1.93709 10.6706 1.98822 10.5941 2.05268 10.5296L3.49373 9.08855C2.6197 8.744 1.91247 8.33062 1.42559 7.87006C0.956591 7.42636 1.26799 6.63816 1.91359 6.63816C2.10629 6.63816 2.28789 6.71896 2.42489 6.85456C2.70009 7.12696 3.19529 7.45886 3.98459 7.77796C5.54429 8.40856 7.73699 8.77016 10.0001 8.77016C12.2632 8.77016 14.4558 8.40856 16.0156 7.77796C16.8049 7.45886 17.3001 7.12696 17.5753 6.85456Z"
                        fill="#fff"></path>
                </svg>
                <i v-else @click="changePwd2" class="fa-solid fa-eye eye2"></i>
            </div>
        </div>

        <!-- <input class="littleInput" type="text">
        <button>獲取驗證碼</button> -->
        <!-- 取消及登入按鈕 -->
        <div class="btnSignarea">

            <button type="button" class="btnSign" @click="navigateToLogin">Cancel</button>
            <button type="button" class="btnSign" @click="register">Sign up</button>

        </div>

    </div>

</template>

<style lang="scss" scoped>
$maincolor: #204163;

//整個區域
.areaSign {
    width: 100vw;
    background-color: $maincolor;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 94px 0;

    //標題文字
    .pageSign {
        color: #f7f8f9;
    }

    // icon + text + input Area
    .inputArea {
        // background-color: #49e246;
        position: relative;

        .fa-envelope {
            font-size: 15pt;
            color: white;
            position: absolute;
            top: 62px;
            left: 12px;
        }

        .lock1 {
            font-size: 14pt;
            color: white;
            position: absolute;
            bottom: 108px;
            left: 12px;
        }

        .lock2 {
            font-size: 14pt;
            color: white;
            position: absolute;
            bottom: 15px;
            left: 12px;
        }

        .fa-eye {
            font-size: 16pt;
            color: white;
            position: absolute;
            bottom: 108px;
            right: 15px;

            &:hover {
                cursor: pointer;
            }
        }

        .closeEye {
            width: 40px;
            height: 25px;
            background-color: $maincolor;
            color: white;
            position: absolute;
            bottom: 105px;
            right: 7.5px;

            &:hover {
                cursor: pointer;
            }

        }

        .eye2 {
            font-size: 16pt;
            color: white;
            position: absolute;
            bottom: 13px;
            right: 15px;

            &:hover {
                cursor: pointer;
            }
        }

        .closeEye2 {
            width: 40px;
            height: 25px;
            background-color: $maincolor;
            color: white;
            position: absolute;
            bottom: 11px;
            right: 7.5px;

            &:hover {
                cursor: pointer;
            }

        }

        //Account & password & Repeat password文字
        .textSign {
            color: white;
            margin-top: 20px;
        }

        //Account及Password及Repeat Password輸入格
        .inputSign {
            width: 400px;
            height: 47px;
            background: $maincolor;
            color: white;
            border: 2px solid white;
            border-radius: 6px;
            padding-left: 40px;
        }
    }

    .littleInput{
        background-color: $maincolor;
        border: 2px solid #f7f8f9;
        border-radius: 4px;
        margin-top: 30px;
    }

    // 取消及登入按鈕
    .btnSignarea {
        display: flex;
        margin-top: 30px;

        .btnSign {
            width: 100px;
            height: 40px;
            color: $maincolor;
            border-radius: 6px;
            margin:  0px 20px;

            &:hover {
                background-color: #77748b;
                color: white;
            }
        }
    }

}
</style>