<template>
    <div>
        <div class='fir'>
            <div class='fitr'>用户名</div> 
            <input placeholder="请输入用户名" v-model="userName"/>
        </div>

        <div class='fir'>
            <div class='fitr'>密码</div>
            <input placeholder="请输入登录密码" v-model="passWord"/>
        </div>

        <div class='fir'>
            <div class='fitr'>登录名(账号)</div>
            <input placeholder="请输入登录名" v-model="userLoginName"/>
        </div>

        <div class='fir'>
            <div class='fitr'>性别</div>
            <input placeholder="请输入性别" v-model="gender"/>
        </div>

        <div class='fir'>
            <div class='fitr'>
                <button v-on:click="submitInfo">提交</button>
            </div>
                
            <a href='http://localhost:8080/#/login'>登录入口</a>
        </div>

        <div>{{response}}</div>
    </div>

</template>
    

<script>
    import axios from 'axios'; 

    export default({
        name: 'register',
        
        data:  function(){
                return {
                    userName: "",
                    passWord: "",
                    userLoginName: "",
                    gender: "",
                    response: ""
                }
            },
        
        methods: {
            submitInfo: function () {
                this.response = null;

                const registerParams = {
                    params: {
                        userName: this.userName,
                        passWord: this.passWord,
                        userLoginName: this.userLoginName,
                        gender: this.gender
                    }
                };

                axios.get("http://localhost:8082/register", registerParams)
                    .then((res)=>{
                        this.response = res.data;
                    })
                    .catch((reason)=>{
                        console.log("register", reason);
                    })
            }
        }
    });
</script>

<style>
    
    .fir{
        display: flex;
        margin: 10px;
    }

    .fitr{
        width: 100px;
        margin: 0 10px 0 10px;
    }
</style>