<template>
    <div>
        <div class='fil'>
            <div class='fitl'>账号</div>
            <input placeholder="请输入账号" v-model="userLoginName"/>
        </div>
        <div class='fil'>
            <div class='fitl'>密码</div>
            <input placeholder="请输入登录密码" v-model="passWord"/>
        </div>

        <div class='fil'>
            <div class='fitl'>
                <button v-on:click="submitInfo">提交</button>
            </div>
            
            <a href='http://localhost:8080/#/register'>注册新用户</a>
        </div>

        <div>{{response}}</div>

    </div>
</template>
    

<script>
     import axios from 'axios'; 
     axios.defaults.withCredentials = true;

     export default({
        name: 'login',

        data:  function(){
                return {
                    userLoginName: "caoguoli372",
                    passWord: "1234567890fjs",
                    response: ""
                }
            },
        
        methods: {
            submitInfo: function() {
                
                this.response = null;

                const loginParams = {
                    params: {
                        userLoginName: this.userLoginName,
                        passWord: this.passWord
                    }
                };

                axios.get("http://localhost:8082/login", loginParams)
                    .then((res)=>{
                        this.response = res.data;

                        if(res.data.success){
                           this.$router.push({path:'/index'});
                           console.log('login', res.data);
                        }
                    })
                    .catch((reason)=>{
                        console.log("login", reason);
                    })
            }
        }
    });
</script>

<style>
    
    .fil{
        display: flex;
        margin: 10px;
    }

    .fitl{
        width: 60px;
        margin: 0 10px 0 10px;
    }
</style>
