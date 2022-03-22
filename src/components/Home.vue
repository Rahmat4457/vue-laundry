<template>
    <div>
        <h1>Hallo Bro</h1>
    </div>
</template>
<script>
module.exports = {
    data: function(){
        return{
            member: []
        }
    },
    methods:{
    getData: function(){
        let config = {
            headers: {
                "Authorization":"Bearer "+this.$cookies.get('Authorization')
            }
        }
        axios.get(base_url + '/member', config)
        .then(response=>{
                console.log(response);
            if(response.data.success == true){
                this.member = response.data.data.member;
        }  
    })
},

    },
mounted(){
    // this.getData();


}
}
</script>
