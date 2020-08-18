<template>
    <el-container>
        <el-header>
            <LoginHeader></LoginHeader>
        </el-header>
        <el-main>
            <el-form :model="signUpForm" status-icon :rules="rules" ref="signUpForm" label-width="120px" class="signUpForm">
                <el-form-item label="Username" prop="username">
                    <el-input v-model="signUpForm.username"></el-input>
                </el-form-item>
                <el-form-item label="Password" prop="password">
                    <el-input type="password" v-model="signUpForm.password" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Check Password" prop="checkPass">
                    <el-input type="password" v-model="signUpForm.checkPass" autocomplete="off"></el-input>
                </el-form-item>
                <el-form-item label="Email" prop="email">
                    <el-input type="email" v-model="signUpForm.email"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitForm('signUpForm')">Sign Up</el-button>
                    <el-button @click="resetForm('signUpForm')">Reset</el-button>
                </el-form-item>
            </el-form>
        </el-main>
    </el-container>
</template>

<script>
    import LoginHeader from "../components/LoginHeader";
    import md5 from 'js-md5';

    export default {
        name: "SignUp",
        components: {LoginHeader},

        data(){
            var checkUsername = (rule, value, callback) => {
                this.$axios.get('/user/getUserByUserName/' + this.signUpForm.username).then((res) => {
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
                    if (this.signUpForm.checkPass !== '') {
                        this.$refs.signUpForm.validateField('checkPass');
                    }
                    callback();
                }
            };
            var validatePass2 = (rule, value, callback) => {
                if (value === '') {
                    callback();
                } else if (value !== this.signUpForm.password) {
                    callback(new Error('The passwords are different!'));
                } else {
                    callback();
                }
            };
            var checkEmail = (rule, value, callback) => {
              const regEmail = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
              if(regEmail.test(this.signUpForm.email)) {
                callback();
              } else {
                callback(new Error('The syntax of email is invalid!'));
              }
            }
            return {
                signUpForm: {
                    username:'',
                    password:'',
                    checkPass:'',
                    email:''
                },
                rules:{
                    username: [
                        { required: true, message: "Please input username!", trigger: 'blur' },
                        { validator: checkUsername, trigger: 'blur'},
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
                const _this = this
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        let user = {
                            "username":this.signUpForm.username,
                            "password":md5(this.signUpForm.password),
                            "email":this.signUpForm.email
                        };
                        this.$axios.post('/user/save', user)
                            .then((res) => {
                                _this.$alert('Sign up Success!', 'Hints', {
                                    confirmButtonText: 'OK',
                                    callback: action => {
                                        _this.$router.push("/blogs")
                                    }
                                })
                            });
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

    .signUpForm{
        max-width: 500px;
        margin: 0 auto;
    }

    body > .el-container {
        margin-bottom: 40px;
    }
</style>