<template>
      <div>
        <el-container>
          <el-header>
            <LoginHeader></LoginHeader>
          </el-header>
          <el-main>
            <el-form :model="ruleForm" status-icon :rules="rules" ref="ruleForm" class="demo-ruleForm">
              <el-form-item label="Username" prop="username">
                <el-input type="text" maxlength="12" v-model="ruleForm.username"></el-input>
              </el-form-item>
              <el-form-item label="Password" prop="password">
                <el-input type="password" v-model="ruleForm.password" autocomplete="off"></el-input>
              </el-form-item>
              <el-form-item>
                <el-button type="primary" @click="submitForm('ruleForm')">Sign in</el-button>
                <el-button type="primary" @click="$router.push('/signup')">Sign up</el-button>
                <el-button @click="resetForm('ruleForm')">Reset</el-button>
              </el-form-item>
            </el-form>
          </el-main>
        </el-container>
      </div>
</template>
<script>
    import LoginHeader from "../components/LoginHeader";

    export default {
        name: 'Login',
        components: {LoginHeader},
        data() {
            var validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('Please input the password!'));
                } else {
                    callback();
                }
            };
            return {
                ruleForm: {
                    password: '',
                    username: ''
                },
                rules: {
                    password: [
                        {validator: validatePass, trigger: 'blur'}
                    ],
                    username: [
                        {required: true, message: 'Please input the username!', trigger: 'blur'},
                        {min: 3, max: 12, message: 'Length must be between 3 and 12!', trigger: 'blur'}
                    ]
                }
            };
        },
        methods: {
            submitForm(formName) {
                const _this = this
                this.$refs[formName].validate((valid) => {
                    if (valid) {
                        // 提交逻辑
                        this.$axios.post('/login', this.ruleForm).then((res)=>{
                            const token = res.headers['authorization']
                            const userInfo = res.data.data
                            _this.$store.commit('SET_TOKEN', token)
                            _this.$store.commit('SET_USERINFO', userInfo)
                            _this.$router.push("/blogs")
                        })
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                });
            },
            resetForm(formName) {
                this.$refs[formName].resetFields();
            }
        },
        mounted() {
            this.$notify({
                title: '看这里：',
                message: '关注公众号：MarkerHub，回复【vueblog】，领取项目资料与源码',
                duration: 1500
            });
        }
    }
</script>
<style>
    .demo-ruleForm{
        max-width: 500px;
        margin: 0 auto;
    }

    .el-header, .el-footer {
        background-color: #B3C0D1;
        color: #333;
        text-align: center;
        line-height: 60px;
    }

    .el-aside {
        background-color: #D3DCE6;
        color: #333;
        text-align: center;
        line-height: 200px;
    }

    .el-main {
        background-color: #E9EEF3;
        color: #333;
        text-align: center;
        line-height: 160px;
    }

    body > .el-container {
        margin-bottom: 40px;
    }

    .el-container:nth-child(5) .el-aside,
    .el-container:nth-child(6) .el-aside {
        line-height: 260px;
    }

    .el-container:nth-child(7) .el-aside {
        line-height: 320px;
    }
</style>
