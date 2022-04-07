<template>
    <div class="container-fluid py-4">
      <div class="row">
        <div class="col-xl-3 col-sm-6 mb-xl-0 mb-4">
          <div class="card">
            <div class="card-body p-3">
              <div class="row">
                <div class="col-8">
                  <div class="numbers">
                    <p class="text-sm mb-0 text-uppercase font-weight-bold">Data User</p>
                    <h5 class="font-weight-bolder">
                      {{ user.length }}
                    </h5>
                  </div>
                </div>
                <div class="col-4 text-end">
                  <div class="icon icon-shape bg-gradient-primary shadow-primary text-center rounded-circle">
                    <i class="fa-solid fa-users"></i>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
module.exports = {
    data: function(){
        return{
            user: []
        }
    },
    methods:{
    getData: function(){
        let config = {
            headers: {
                "Authorization":"Bearer "+this.$cookies.get('Authorization')
            }
        }
        axios.get(base_url + '/user', config)
        .then(response=>{
                console.log(response);
            if(response.data.success == true){
                this.user = response.data.data.user;
        }  
    })
},

    },
mounted(){
     this.getData();


}
}
</script>