<template>
    <el-container>
        <el-header>
            <LoginHeader></LoginHeader>
        </el-header>
        <el-main>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" label-width="120px" class="demo-ruleForm">
                <el-form-item label="Username" prop="username">
                    <el-input v-model="ruleForm.username"></el-input>
                </el-form-item>
                <el-form-item label="Password" prop="password">
                    <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Check Password" prop="checkPass">
                    <el-input type="password" v-model="ruleForm.checkPass" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Email" prop="email">
                    <el-input type="email" v-model="ruleForm.email"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitForm('ruleForm')">Sign Up</el-button>
                    <el-button @click="resetForm('ruleForm')">Reset</el-button>
                </el-form-item>
            </el-form>
        </el-main>
    </el-container>
</template>

<script>
    import LoginHeader from "../components/LoginHeader";

    export default {
        name: "SignUp",
        components: {LoginHeader},

        data(){
            var checkUsername = (rule, value, callback) => {
                this.$axios.get('/user/getUserByUserName/' + this.ruleForm.username).then((res) => {
                    let existed = res.data.data;
                    if(existed === true) {
                        callback(new Error('The username is already used!'));
                    } else {
                        callback();
                    }
                });

            };
            var validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback();
                } else {
                    if (this.ruleForm.checkPass !== '') {
                        this.$refs.ruleForm.validateField('checkPass');
                    }
                    callback();
                }
            };
            var validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback();
                } else if (value !== this.ruleForm.password) {
                    callback(new Error('The passwords are different!'));
                } else {
                    callback();
                }
            };
            var checkEmail = (rule, value, callback) => {
              const regEmail = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
              if(regEmail.test(this.ruleForm.email)) {
                callback();
              } else {
                callback(new Error('The syntax of email is invalid!'));
              }
            }
            return {
                ruleForm: {
                    username:'',
                    password:'',
                    checkPass:'',
                    email:''
                },
                rules:{
                    username: [
                        { validator: checkUsername, trigger: 'blur'},
                        { required: true, message: "Please input username!", trigger: 'blur' },
                        { min: 3, max: 12, message: 'Length must be between 3 and 12!', trigger: 'blur'}
                    ],
                    password: [
                        { required: true, message: "Please input password!", trigger: 'blur' },
                        { validator: validatePass, trigger: 'blur' }
                    ],
                    checkPass: [
                        { required: true, message: "Please input password again!", trigger: 'blur' },
                        { validator: validatePass2, trigger: 'blur' }
                    ],
                    email: [
                        { validator: checkEmail, trigger: 'blur'},
                        { required: true, message: "Please input email!", trigger: 'blur' }
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        alert('submit!');
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        }
    }
</script>

<style scoped>
    .el-container:nth-child(5) .el-aside,
    .el-container:nth-child(6) .el-aside {
        line-height: 260px;
    }

    .el-container:nth-child(7) .el-aside {
        line-height: 320px;
    }

    .demo-ruleForm{
        max-width: 500px;
        margin: 0 auto;
    }

    body > .el-container {
        margin-bottom: 40px;
    }
</style>