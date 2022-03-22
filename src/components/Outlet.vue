<template>
    <div>
        <main>
            <div class="container-fluid py-4">
      <div class="row">
        <div class="col-12">
          <div class="card mb-4">
            <div class="card-header pb-0">
              <h1>Outlet</h1>
            </div>
            <div class="card-body px-0 pt-0 pb-2">
              <div class="table-responsive p-4">
                  <a class="btn btn-info btn-sm mb-0" v-b-modal.modal_outlet @click=Add()>Tambah</a>
                <table class="table align-items-center mb-0">
                                <tr>
                                    <td>ID Outlet</td>
                                    <td>Nama Outlet</td>
                                    <td>Aksi</td>
                                </tr>
                                <tr v-for="tlet in outlet" :key="tlet">
                                    <td>{{ tlet.id_outlet }}</td>
                                    <td>{{ tlet.nama_outlet }}</td>
                                    <td>
                                        <a v-b-modal.modal_outlet href="#" class="btn btn-success" @click="Edit(tlet)">Ubah</a>
                                        <a href="#" class="btn btn-danger" @click="Delete(tlet.id_outlet)">Hapus</a>
                                    </td>
                                </tr>
                            </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>

        <b-modal id="modal_outlet" ref="modal" title="Form Outlet" size="md" @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama_outlet" placeholder="masukkan outlet" id="inputOutlet" class="form-control" type="text"/>
                    <label for="inputOutlet">Outlet</label>
                </div>
            </form>
        </b-modal>
    </div>
</template>
<script>
module.exports = {
    data: function(){
        return{
            nama_outlet:"",
            outlet:[],
            action:""
            }
        },
    methods:{
        getData: function(){
        let config = {
            headers: {
                "Authorization":"Bearer "+this.$cookies.get('Authorization')
            }
        }
        axios.get(base_url + '/outlet', config)
        .then(response=>{
                console.log(response);
            if(response.data.success == true){
                this.outlet = response = response.data.data.outlet;
        }  
    })
},
Add: function(){
    this.action = "insert";
    this.nama_outlet = "";

},
Edit: function(item){
    this.action = "update";
    this.nama_outlet = item.nama_outlet;
},

Save: function(){
    let config = {
        headers : {
            "Authorization":"Bearer "+this.$cookies.get('Authorization')
        }
    }

    let form = {
        "nama_outlet" : this.nama_outlet,
    }
    if(this.action == "insert"){
        axios.post(base_url + '/outlet', form, config)
        .then( response => {
            Swal.fire(response.data.message)
    })
    }else{
        axios.put(base_url + '/outlet/'+this.id_outlet, form, config)
        .then(response =>{
            Swal.fire(response.data.message)
        })
    }
    this.getData();
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
            axios.delete(base_url + '/outlet/'+ id, config)
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
    }
}
</script>
