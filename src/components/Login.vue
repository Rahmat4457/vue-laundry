<template>
    <main class="main-content  mt-0">
    <section>
      <div class="page-header min-vh-100">
        <div class="container">
          <div class="row">
            <div class="col-xl-4 col-lg-5 col-md-7 d-flex flex-column mx-lg-0 mx-auto">
              <div class="card card-plain">
                
                <div class="card-header">
                  <div class="pb-0 text-start">
                  <h4 class="font-weight-bolder">Masuk</h4>
                  <p class="mb-3">Inputkan username dan password untuk masuk</p>
                </div>
                  <form role="form" v-on:submit.prevent="Login">
                    <div class="mb-3">
                      <input v-model="username" type="name" class="form-control form-control-lg" placeholder="username" aria-label="username">
                    </div>
                    <div class="mb-3">
                      <input v-model="password" type="password" class="form-control form-control-lg" placeholder="Password" aria-label="Password">
                    </div>
                    <div class="text-center">
                      <button type="submit" class="btn btn-lg btn-primary btn-lg w-100 mt-4 mb-0">Masuk</button>
                    </div>
                  </form>
                </div>
              </div>
            </div>
            <div class="col-6 d-lg-flex d-none h-100 my-auto pe-0 position-absolute top-0 end-0 text-center justify-content-center flex-column">
              <div class="position-relative bg-gradient-primary h-100 m-3 px-7 border-radius-lg d-flex flex-column justify-content-center overflow-hidden" style="background-image: url('https://images.unsplash.com/photo-1545173168-9f1947eebb7f?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8bGF1bmRyeXxlbnwwfHwwfHw%3D&w=1000&q=80');
                background-size: cover;">
                <span class="mask bg-gradient-primary opacity-6"></span>
                <h4 class="mt-5 text-white font-weight-bolder position-relative">"Perhatian!!!!!!!"</h4>
                <p class="text-white position-relative">Jangan cuci baju anda sendiri, cucilah di laundry kami</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
module.exports ={
    data: function(){
        return {
            username:"",
            password:"",
            message: "",
        }
    },
        methods: {
            Login: function(){
                let timerInterval
                Swal.fire({
                    title: 'Loading',
                    timer: 2000,
                didOpen: () => {
                    Swal.showLoading()
                    const b = Swal.getHtmlContainer().querySelector('b')
                    timerInterval = setInterval(() => {
                    b.textContent = Swal.getTimerLeft()
                    }, 100)
                        },
                willClose: () => {
                    clearInterval(timerInterval)
                    
                
                let form = {
                    "username": this.username,
                    "password": this.password
                }

                axios.post(base_url + '/login',form)
                .then(response =>{
                    if (response.data.success == true) {
                        Swal.fire(response.data.message)

                        if(this.$cookies.isKey('Authorization')){
                            this.$cookies.remove('Authorization');
                        }
                        this.$cookies.set('Authorization', response.data.data.token);
                        
                        location.reload();
                    }else{
                        Swal.fire(response.data.message)
                    }
                    
                })

            },
        })
            }
        }
    }

</script>
