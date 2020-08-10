<template>
    <el-container>
        <el-header>
            <LoginHeader></LoginHeader>
        </el-header>
        <el-main>Sign Up</el-main>
    </el-container>
</template>

<script>
    import LoginHeader from "../components/LoginHeader";
    export default {
        name: "SignUp",
        components: {LoginHeader}
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
</style>