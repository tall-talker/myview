<template>

    <div>
        <headerComponent></headerComponent>

        <div id='top2' style='margin: 20px 0 20px 0'>
            主页
            <a href='http://localhost:8080/#/notify' style='margin: 0 20px 0 20px'>公告板</a>
        </div>

        <div style='margin-bottom: 20px'>
            修改密码 <input placeholder="请输入新密码" v-model="password"/>

            <button v-on:click='submitPassword' style='margin-left: 20px'>提交</button>
        </div>

        <div style='margin-bottom: 20px'>
            修改用户名 <input placeholder="请输入新用户名" v-model="userName"/>

            <button v-on:click='submitUserName' style='margin-left: 20px'>提交</button>
        </div>

        ===================================================

        <div id='main' v-for="item in response">
            <div style='margin-right: 20px'>用户名：{{ item.userName }}</div>
            <div>性别：{{ item.gender }}</div>
            <button id='delBtn' v-on:click="deleteUser(item)" v-if="!checkAdmin(item)">删除</button>
        </div>

        ===================================================
    </div>

</template>
    

<script>
    import axios from 'axios'; 
    import headerComponent from './header';

    export default({
        name: 'index',

        components: {
            headerComponent
        },

        data:  function(){
                return {
                    response: [],
                    user: {},
                    password: '',
                    userName: ''
                }
            },
        
        created: function () {
            this.getUserList();
        },

        methods: {
            checkAdmin: function(item) {
               return item.userName == 'caoguoli';
            },
            
            getUserList:  function() {
                axios.get("http://localhost:8082/getAllUsers")
                    .then((res)=>{
                        console.log("getUserList", res.data);

                        if(res.data.success){
                            this.response = res.data.data;
                        }else{
                            alert(res.data.message);
                        }
                    }) 
            },

            deleteUser: function(item) {
                console.log('deleteUser', item);

                const deleteParams = { params: { id: item.id } };

                axios.get("http://localhost:8082/deleteUser", deleteParams)
                    .then((res) =>{

                        if(res.data.success){
                            this.getUserList();
                        }

                        console.log("delete", res.data);
                    }) 
            },

            submitPassword: function(){
                const passwordParams = { params: { password: this.password } };

                axios.get("http://localhost:8082/updatePassword", passwordParams)
                    .then((res) =>{
                        console.log("submitPassword", res.data);

                        if(res.data.success){
                            this.$router.push({path:'/login'})
                        }
                    }) 
            },

            submitUserName: function(){
                const userNameParams = { params: { userName: this.userName } };

                axios.get("http://localhost:8082/updateUserName", userNameParams)
                    .then((res) =>{
                        console.log("submitUserName", res.data);
                    }) 
            }
        }
    });
</script>

<style>
  #main, #top2{
    margin: 10px;
    display: flex;
  }

  #delBtn {
    margin-left: 20px;
  }

  #top2 {
    margin: 20px 0 20px 0
  }

</style>
