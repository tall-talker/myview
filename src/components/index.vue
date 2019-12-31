<template>
    <div>
        <p>用户列表</p>

        <div id='main' v-for="item in response">
            <div style='margin-right: 20px'>用户名：{{ item.userName }}</div>
            <div>性别：{{ item.gender }}</div>
            <button id='delBtn' v-on:click="deleteUser(item)" v-if="!checkAdmin(item)">删除</button>
        </div>

        <p><button id='delBtn' v-on:click="logout">退出</button></p>

        <p></p>
    </div>

</template>
    

<script>
    import axios from 'axios'; 

    export default({
        name: 'index',

        data:  function(){
                return {
                    response: []
                }
            },
        
        created: function () {
            this.getList();
        },

        methods: {
            checkAdmin: function(item) {
               return item.userName == 'caoguoli';
            },
            
            getList:  function() {
                axios.get("http://localhost:8082/getAllUsers")
                    .then((res)=>{

                        if(res.data.success){
                            this.response = res.data.data.map(
                                function(obj){
                                    return {'userName': obj.userName, 'gender': obj.gender}
                            });
                        }else{
                            alert(res.data.message);
                        }

                        console.log("getList", res.data);
                    }) 
            },

            deleteUser: function(item) {
                console.log('deleteUser', item);

                const deleteParams = {
                    params: {
                        userName: item.userName,
                    }
                };

                axios.get("http://localhost:8082/deleteUser", deleteParams)
                    .then(function(res){

                        if(res.data.success){
                            this.getList();
                        }

                        console.log("delete", res.data);
                    }) 
            },

            logout: function() {

                axios.get("http://localhost:8082/logout").then(
                    (res)=>{ 
                        console.log("delete", res.data); 
                        this.$router.push({path:'/login'})
                }) 
            }
        }
    });
</script>

<style>
  #main {
    margin: 10px;
    display: flex;
  }

  #delBtn {
    margin-left: 20px;
  }
</style>
