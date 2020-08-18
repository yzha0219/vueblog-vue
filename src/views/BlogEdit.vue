<template>
    <div class="m-container">
        <Header></Header>
        <div class="m-content">
            <el-form ref="editForm" status-icon :model="editForm" :rules="rules" label-width="80px">
                <el-form-item label="Title" prop="title">
                    <el-input v-model="editForm.title"></el-input>
                </el-form-item>
                <el-form-item label="Description" prop="description">
                    <el-input type="textarea" v-model="editForm.description"></el-input>
                </el-form-item>
                <el-form-item label="Content" prop="content">
                    <mavon-editor v-model="editForm.content" placeholder="Start edit...."/>
                </el-form-item>
                <el-form-item>
                    <el-button type="primary" @click="submitForm()">create</el-button>
                    <el-button>cancel</el-button>
                </el-form-item>
            </el-form>
        </div>
    </div>
</template>

<script>
    import Header from "../components/Header";

    export default {
        name: "BlogEdit",
        components: {Header},
        data() {
            return {
                editForm: {
                    id: null,
                    title: '',
                    description: '',
                    content: ''
                },
                rules: {
                    title: [
                        {required: true, message: 'Please input title!', trigger: 'blur'},
                        {min: 3, max: 50, message: 'The length should be between 3 and 50 characters!', trigger: 'blur'}
                    ],
                    description: [
                        {required: true, message: 'Please input description!', trigger: 'blur'}
                    ]
                }
            }
        },
        created() {
            const blogId = this.$route.params.blogId
            const _this = this
            if (blogId) {
                this.$axios.get('/blog/' + blogId).then((res) => {
                    const blog = res.data.data
                    _this.editForm.id = blog.id
                    _this.editForm.title = blog.title
                    _this.editForm.description = blog.description
                    _this.editForm.content = blog.content
                });
            }
        },
        methods: {
            submitForm() {
                const _this = this
                this.$refs.editForm.validate((valid) => {
                    if (valid) {
                        this.$axios.post('/blog/edit', this.editForm, {
                            headers: {
                                "Authorization": localStorage.getItem("token")
                            }
                        }).then((res) => {
                            _this.$alert('operation success', 'Hints', {
                                confirmButtonText: 'OK',
                                callback: action => {
                                    _this.$router.push("/blogs")
                                }
                            });
                        });
                    } else {
                        console.log('error submit!!');
                        return false;
                    }
                })
            }
        }
    }
</script>

<style scoped>
</style>