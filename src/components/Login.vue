<template>
    <div class="login_container">
        <div class="login_box">
            <div class="avatar_box">
                <img src="../assets/logo.png" alt="">
            </div>
            <el-form ref="loginFormRef" :model="loginForm" :rules="rules" label-width="80px" class="login_form">
                <el-form-item label-width="0px" prop="username">
                    <el-input prefix-icon="iconfont icon-user" v-model="loginForm.username"></el-input>
                </el-form-item>
                <el-form-item label-width="0px" prop="password">
                    <el-input type="password" prefix-icon="iconfont icon-3702mima" v-model="loginForm.password"></el-input>
                </el-form-item>
                <el-form-item label-width="0px" class="btns">
                    <el-button type="primary" @click="submitForm">登录</el-button>
                    <el-button type="info" @click="resetLoginForm">重置</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    export default {
        name: "Login",
        data(){
            return {
                loginForm:{
                    username: "admin",
                    password: "123456"
                },
                rules: {
                    username: [
                        { required: true, message: '请输入用户名', trigger: 'blur' },
                        { min: 3, max: 32, message: '长度在 3 以上', trigger: 'blur' }
                    ],
                    password: [
                        { required: true, message: '请输入密码', trigger: 'blur' },
                        { min: 3, message: '长度在 3 到 5 个字符', trigger: 'blur' }
                    ],
                    region: [
                        { required: true, message: '请选择活动区域', trigger: 'change' }
                    ],
                    date1: [
                        { type: 'date', required: true, message: '请选择日期', trigger: 'change' }
                    ],
                    date2: [
                        { type: 'date', required: true, message: '请选择时间', trigger: 'change' }
                    ],
                    type: [
                        { type: 'array', required: true, message: '请至少选择一个活动性质', trigger: 'change' }
                    ],
                    resource: [
                        { required: true, message: '请选择活动资源', trigger: 'change' }
                    ],
                    desc: [
                        { required: true, message: '请填写活动形式', trigger: 'blur' }
                    ]
                }
            }
        },
        methods:{
            resetLoginForm(){
                this.$refs['loginFormRef'].resetFields();
            },
            submitForm(){
                this.$refs['loginFormRef'].validate(async (valid) => {
                    if(!valid){
                        return;
                    }

                    const {data: res} = await this.$http.post("login", this.loginForm);
                    if(res.meta.status !== 200){
                        this.$message.error("登录失败");
                        return;
                    }

                    this.$message.success("登录成功");

                    console.log(res);
                    window.sessionStorage.setItem("token", res.data.token);
                    this.$router.push("/home")
                    return;

                })
            }
        }

    }
</script>

<style lang="less" scoped>

    .login_container{
        background-color: #2b4b6b;
        height: 100%;
        width: 100%;
    }

    .login_box{
        width: 450px;
        height: 300px;
        background-color: #ffffff;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        border-radius: 10px;
    }

    .avatar_box{
        height: 130px;
        width: 130px;
        border: 1px solid #eee;
        border-radius: 50%;
        position: absolute;
        left: 50%;
        transform: translate(-50%, -50%);
        padding: 10px;
        box-shadow: 0 0 10px #ddd;

        img{
            height: 100%;
            width: 100%;
            border-radius: 50%;
            background-color: #ffffff;

        }
    }

    .btns{
        display: flex;
        justify-content: flex-end;
    }

    .login_form{
        position: absolute;
        bottom: 0px;
        width: 100%;
        padding: 10px;
        box-sizing: border-box;
    }



</style>