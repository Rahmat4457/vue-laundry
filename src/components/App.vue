<template>
<div class="min-height-300 bg-primary position-absolute w-100">
<div class="g-sidenav-show bg-gray-100">
    <div class="min-height-300 bg-primary position-absolute w-100"></div>
  <aside class="sidenav bg-white navbar navbar-vertical navbar-expand-xs border-0 border-radius-xl my-3 fixed-start ms-4 " id="sidenav-main">
    <div class="sidenav-header">
      <i class="fas fa-times p-3 cursor-pointer text-secondary opacity-5 position-absolute end-0 top-0 d-none d-xl-none" aria-hidden="true" id="iconSidenav"></i>
      <a class="navbar-brand m-0" href=" https://demos.creative-tim.com/argon-dashboard/pages/dashboard.html " target="_blank">
        <span class="ms-1 font-weight-bold">Laundry Rahmat</span>
      </a>
    </div>    <hr class="horizontal dark mt-0">
    <div class="collapse navbar-collapse  w-auto " id="sidenav-collapse-main">
      
      <ul class="navbar-nav">
        <li class="nav-item">
          <router-link to="/" class="nav-link collapsed">
                <div class="sb-nav-link-icon">
                    <i class="fas fa-columns"></i>
                    <span class="nav-link-text ms-1">Home</span>
                </div>
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/member" class="nav-link collapsed" v-if="role !== 'owner'">
                <div class="sb-nav-link-icon">
                    <i class="fas fa-columns"></i>
                    <span class="nav-link-text ms-1">Member</span>
                </div>
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/User" class="nav-link collapsed" v-if="role === 'admin'">
                <div class="sb-nav-link-icon">
                    <i class="fas fa-columns"></i>
                    <span class="nav-link-text ms-1">User</span>
                </div>
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/Outlet" class="nav-link collapsed" v-if="role === 'admin'">
                <div class="sb-nav-link-icon">
                    <i class="fas fa-columns"></i>
                    <span class="nav-link-text ms-1">Outlet</span>
                </div>
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/Paket" class="nav-link collapsed" v-if="role === 'admin'">
                <div class="sb-nav-link-icon">
                    <i class="fas fa-columns"></i>
                    <span class="nav-link-text ms-1">Paket</span>
                </div>
          </router-link>
        </li>
        <li class="nav-item">
          <router-link to="/transaksi" class="nav-link collapsed">
                <div class="sb-nav-link-icon">
                    <i class="fas fa-columns"></i>
                    <span class="nav-link-text ms-1">Transaksi</span>
                </div>
          </router-link>
        </li>
      </ul>
    </div>
  </aside>
  <main class="main-content position-relative border-radius-lg ">
        <nav class="navbar navbar-main navbar-expand-lg px-0 mx-4 shadow-none border-radius-xl " id="navbarBlur" data-scroll="false">
      <div class="container-fluid py-1 px-3">
        <nav aria-label="breadcrumb">
          <h6 class="font-weight-bolder text-white mb-0"></h6>
        </nav>
        <div class="collapse navbar-collapse mt-sm-0 mt-2 me-md-0 me-sm-4" id="navbar">
          <div class="ms-md-auto pe-md-3 d-flex align-items-center">
          </div>
          <ul class="navbar-nav  justify-content-end">
            <li class="nav-item dropdown pe-2 d-flex align-items-center">
              <a href="javascript:;" class="nav-link text-white p-0" id="dropdownMenuButton" data-bs-toggle="dropdown" aria-expanded="false">
                <i class="fa-solid fa-user"></i>
              </a>
              <ul class="dropdown-menu  dropdown-menu-end  px-2 py-3 me-sm-n4" aria-labelledby="dropdownMenuButton">
                    <div class="d-flex py-1">
                      <div class="d-flex flex-column justify-content-center">

                        <h6 class="text-sm font-weight-normal mb-1">
                          <span class="font-weight-bold">
                            <a @click="Logout">Logout</a>
                          </span>
                        </h6>
                      </div>
                    </div>
              </ul>
            </li>
          </ul>
        </div> 
      </div>
    </nav>
      <router-view></router-view>
  </main>
</div>
</div>

</template>
<script>
module.exports = {
    data: function(){
        return{
            username:"",
            role : "",
            }
        },
    methods:{
        getData: function(){
        let config = {
            headers: {
                "Authorization":"Bearer "+this.$cookies.get('Authorization')
            }
        }
        axios.get(base_url + '/login/check', config)
        .then(response=>{
            if(response.data.success == true){
                this.username = response.data.data.username;
                this.role = response.data.data.role;
        }  
    })
},

    Logout: function(){
        this.$cookies.remove("Authorization");
        this.componentName = "login";

        window.location == front_url;
        location.reload();
    }
},
mounted(){
    this.getData();
},
}
    
</script>