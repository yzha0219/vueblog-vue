<template>
    <div class="m-content">
        <h3>Welcome to MarkerHub Blogs</h3>
        <div class="block">
            <el-avatar :size="50" :src="user.avatar"></el-avatar>
            <div>{{ user.username }}</div>
        </div>
        <div class="maction">
            <el-link href="/blogs">Home</el-link>
            <el-divider direction="vertical"></el-divider>
            <span>
          <el-link type="success" href="/blog/add" :disabled="!hasLogin">Post Blog</el-link>
        </span>
            <el-divider direction="vertical"></el-divider>
            <span>
                <el-link type="warning" href="/user/edit" :disabled="!hasLogin">Profile Edit</el-link>
            </span>
            <el-divider direction="vertical"></el-divider>
            <span v-show="!hasLogin">
          <el-link type="primary" href="/login">Sign in</el-link>
        </span>
            <span v-show="hasLogin">
          <el-link type="danger" @click="logout">Logout</el-link>
        </span>
        </div>
    </div>
</template>
<script>
    import LoginHeader from "./LoginHeader";
    export default {
        name: "Header",
        components: {LoginHeader},
        data() {
            return {
                hasLogin: false,
                user: {
                    username: 'Please Login First!',
                    avatar: "https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png"
                },
                blogs: {},
                currentPage: 1,
                total: 0
            }
        },
        methods: {
            logout() {
                const _this = this
                console.log(localStorage.getItem("token"))
                this.$axios.get('/logout', {
                    headers: {
                        "Authorization": localStorage.getItem("token")
                    }
                }).then((res) => {
                    _this.$store.commit('REMOVE_INFO')
                    _this.$router.push('/login')
                })
            }
        },
        created() {
            if (this.$store.getters.getUser.username) {
                this.user.username = this.$store.getters.getUser.username
                this.user.avatar = this.$store.getters.getUser.avatar
                this.hasLogin = true
            }
        }
    }
</script>

<style scoped>
    .m-content {
        max-width: 960px;
        margin: 0 auto;
        text-align: center;
    }

    .maction {
        margin: 10px 0;
    }
</style>
