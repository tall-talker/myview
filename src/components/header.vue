<template>
	<div>
	    当前用户：{{user.userName}}

	    <button v-on:click="logout" style='margin-left: 20px'>退出登录</button>

	    <div></div>
	</div>

</template>

<script>
    import axios from 'axios'; 

    export default({

        data:  function(){
                return {
                    
                    user: {}
                }
            },
        
        created: function (){
            
            this.refreshAccount();
        },

        methods: {

		        refreshAccount: function(){

		            axios.get("http://localhost:8082/refreshAccount")
		                .then((res)=>{
		                    console.log("refreshAccount", res.data);

		                    if(res.data.success){
		                        global.user = res.data.data;
		                        this.user = res.data.data;
		                    }
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