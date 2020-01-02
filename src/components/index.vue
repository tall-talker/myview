<template>

    <div>
        <headerComponent></headerComponent>

        <div id='top2' style='margin: 20px 0 20px 0'>
            主页
            <a href='http://localhost:8080/#/notify' style='margin: 0 20px 0 20px'>公告板</a>
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
    import headerComponent from './header.vue';

    export default({
        name: 'index',

        components: {
            headerComponent
        },

        data:  function(){
                return {
                    response: [],
                    user: {}
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
                        console.log("getList", res.data);

                        if(res.data.success){
                            this.response = res.data.data.map(
                                function(obj){
                                    return {'userName': obj.userName, 'gender': obj.gender}
                            });
                        }else{
                            alert(res.data.message);
                        }
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
