<template>
    <div class="m-container">
        <Header></Header>
        <div class="m-content">
            <el-form :model="userEditForm" status-icon :rules="rules" ref="userEditForm" label-width="120px"
                     class="userEditForm">
                <el-form-item label="Username" prop="username">
                    <el-input v-model="userEditForm.username"></el-input>
                </el-form-item>
                <el-form-item label="Email" prop="email">
                    <el-input type="email" v-model="userEditForm.email"></el-input>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitForm('userEditForm')">Commit</el-button>
                    <el-button @click="resetForm('userEditForm')">Reset</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import Header from "../components/Header";

    export default {
        name: "UserEdit",
        components: {Header},

        data() {
            var checkUsername = (rule, value, callback) => {
                this.$axios.get('/user/getUserByUserName/' + this.userEditForm.username).then((res) => {
                    let existed = res.data.data;
                    if (existed === true) {
                        callback(new Error('The username is already used!'));
                    } else {
                        callback();
                    }
                });

            };
            var checkEmail = (rule, value, callback) => {
                const regEmail = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/;
                if (regEmail.test(this.userEditForm.email)) {
                    callback();
                } else {
                    callback(new Error('The syntax of email is invalid!'));
                }
            }
            return {
                userEditForm: {
                    username: '',
                    email: ''
                },
                rules: {
                    username: [
                        {required: true, message: "Please input username!", trigger: 'blur'},
                        {validator: checkUsername, trigger: 'blur'},
                        {min: 3, max: 12, message: 'Length must be between 3 and 12!', trigger: 'blur'}
                    ],
                    email: [
                        {validator: checkEmail, trigger: 'blur'},
                        {required: true, message: "Please input email!", trigger: 'blur'}
                    ]
                }
            }
        },
        methods: {
            submitForm(formName) {
                this.$refs[formName].validate((valid) => {

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

    .userEditForm {
        max-width: 500px;
        margin: 0 auto;
    }

    body > .el-container {
        margin-bottom: 40px;
    }
</style>