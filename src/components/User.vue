<template>
    <div>
        <main>
            <div class="container-fluid py-4">
      <div class="row">
        <div class="col-12">
          <div class="card mb-4">
            <div class="card-header pb-0">
              <h1>User</h1>
            </div>
            <div class="card-body px-0 pt-0 pb-2">
              <div class="table-responsive p-4">
                  <a class="btn btn-info btn-sm mb-0" v-b-modal.modal_member @click=Add()>Tambah</a>
                <table class="table align-items-center mb-0">
                                <tr>
                                    <td>ID User</td>
                                    <td>Nama</td>
                                    <td>Username</td>
                                    <td>role</td>
                                    <td>action</td>
                                </tr>
                                <tr v-for="ser in user" :key="ser">
                                    <td>{{ ser.id }}</td>
                                    <td>{{ ser.name }}</td>
                                    <td>{{ ser.username }}</td>
                                    <td>{{ ser.role }}</td>
                                    <td>
                                        <a v-b-modal.modal_user href="#" class="btn btn-success" @click="Edit(ser)">Ubah</a>
                                        <a href="#" class="btn btn-danger" @click="Delete(ser.id)">Hapus</a>
                                    </td>
                                </tr>
                            </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>

        <b-modal id="modal_user" ref="modal" title="Form User" size="md" @ok="Save">
            <form>
                
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="name" placeholder="masukkan name" id="inputNama" class="form-control" type="text"/>
                    <label for="inputNama">Nama</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="username" placeholder="masukkan username"  id="inputUsername" class="form-control" type="text"/>
                    <label for="inputUsername">Username</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="password" placeholder="masukkan password"  id="inputPassword" class="form-control" type="password"/>
                    <label for="inputUsername">Password</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    
                    <select id="inputRole" class="form-control" v-model="role">
                    <option value="">--Pilih Role--</option>
                    <option value="admin">Admin</option>
                    <option value="kasir">Kasir</option>
                    <option value="owner">Owner</option>
                    </select>
                    <label for="inputRole" >Role</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    
                    <b-form-select class="form-control" v-model="id_outlet" :options="data_outlet"></b-form-select>
                    <label for="id_outlet">Outlet</label>
                </div>
            </form>
        </b-modal>

    </div>
</template> 
<script>
module.exports = {
    data: function(){
        return{
            id_outlet:"",
            name:"",
            username : "",
            password:"",
            role : "",
            action:"",
            user:[],
            data_outlet:[],
            fields : ["id","name","username","role","outlet","aksi"],
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
                this.user = response = response.data.data.user;
        }   else{
            this.componentName = 'login';
            window.location = front_url;
        }  
    })
    .catch(error =>{
        console.log(error);
    });
},
OutletDropdown: function(){
    let config= {
        headers:{
            "Authorization":"Bearer "+this.$cookies.get('Authorization')
        }
    };
    axios.get(base_url + '/outlet', config)
        .then(response=>{
            let json_outlet = response.data.data.outlet;
            let list_outlet = [{
                value: "", text:"--Pilih Outlet--"
            }]
            json_outlet.forEach(element =>{
                list_outlet.push({
                    value: element.id_outlet, text: element.nama_outlet
                    })
                });
                this.data_outlet = list_outlet
            })
        },

Add: function(){
    this.action = "insert";
    this.id_outlet = "";
    this.id = "";
    this.name = "";
    this.username = "";
    this.password = "";
    this.role = "";
},
Edit: function(item){
    this.action = "update";
    this.id_outlet = item.id_outlet;
    this.id = item.id
    this.name = item.name;
    this.username = item.username;
    this.role = item.role;
},

Save: function(){
    let config = {
        headers : {
            "Authorization":"Bearer "+this.$cookies.get('Authorization')
        }
    }

    let form = {
        "id_outlet" : this.id_outlet,
        "name" : this.name,
        "alamat" : this.alamat,
        "username": this.username,
        "password": this.password,
        "role": this.role,
    }
    if(this.action == "insert"){
        axios.post(base_url + '/user', form, config)
        .then( response => {
            this.getData();
            Swal.fire(response.data.message)
    })
    .catch(error => {
          console.log(error);
        });
    }else{
        axios.put(base_url + '/user/'+this.id, form, config)
        .then(response =>{
            this.getData();
            Swal.fire(response.data.message)
        })
        .catch(error => {
          console.log(error);
        });
    }

},
    Delete: function(id){
        Swal.fire({
        title: 'Apakah anda yakin? ',
        text: "Data akan hilang secara permanen!!!",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Ya, Hapus!'
    })
       .then((result) => {
  if (result.isConfirmed) {
           let config = {
               headers : {
                   "Authorization": "bearer " + this.$cookies.get('Authorization')
               }
           }
            axios.delete(base_url + '/user/'+ id, config)
            .then(response =>{
            Swal.fire(response.data.message)
        })
        this.getData();
       }
       })
    }
    },
    mounted(){
        this.getData();
        this.OutletDropdown();
    }
}
    
</script>