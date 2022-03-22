<template>
    <div>
        <main>
            <div class="container-fluid py-4">
      <div class="row">
        <div class="col-12">
          <div class="card mb-4">
            <div class="card-header pb-0">
              <h1>Member</h1>
            </div>
            <div class="card-body px-0 pt-0 pb-2">
              <div class="table-responsive p-4">
                  <a class="btn btn-info btn-sm mb-0" v-b-modal.modal_member @click=Add()>Tambah</a>
                <table class="table align-items-center mb-0">
                                <tr>
                                    <td>ID Paket</td>
                                    <td>Jenis</td>
                                    <td>Harga</td>
                                    <td>Aksi</td>
                                </tr>
                                <tr v-for="ket in paket" :key="ket">
                                    <td>{{ ket.id_paket }}</td>
                                    <td>{{ ket.jenis }}</td>
                                    <td>{{ ket.harga }}</td>
                                    <td>
                                        <a v-b-modal.modal_paket href="#" class="btn btn-success" @click="Edit(ket)">Ubah</a>
                                        <a href="#" class="btn btn-danger" @click="Delete(ket.id_paket)">Hapus</a>
                                    </td>
                                </tr>
                            </table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </main>

        <b-modal id="modal_paket" ref="modal" title="Form Paket" size="md" @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="jenis" placeholder="masukkan paket" id="inputJenis" class="form-control" type="text"/>
                    <label for="inputJenis">Jenis</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="harga" placeholder="Masukkan harga" id="inputHarga" class="form-control" type="text"/>
                    <label for="inputHarga">Harga</label>
                </div>
            </form>
        </b-modal>
    </div>
</template>
<script>
module.exports = {
    data: function(){
        return{
            jenis:"",
            harga : "",
            paket :[],
            action :""

            }
        },
    methods:{
        getData: function(){
        let config = {
            headers: {
                "Authorization":"Bearer "+this.$cookies.get('Authorization')
            }
        }
        axios.get(base_url + '/paket', config)
        .then(response=>{
                console.log(response);
            if(response.data.success == true){
                this.paket = response = response.data.data.paket;
        }  
    })
},
Add: function(){
    this.action = "insert";
    this.jenis = "";
    this.harga = "";
    this.id_paket = "";

},
Edit: function(item){
    this.action = "update";
    this.jenis = item.jenis;
    this.harga = item.harga;
    this.id_paket = item.id_paket;
},

Save: function(){
    let config = {
        headers : {
            "Authorization":"Bearer "+this.$cookies.get('Authorization')
        }
    }

    let form = {
        "jenis" : this.jenis,
        "harga": this.harga,
    }
    if(this.action == "insert"){
        axios.post(base_url + '/paket', form, config)
        .then( response => {
            Swal.fire(response.data.message)
    })
    }else{
        axios.put(base_url + '/paket/'+this.id_paket, form, config)
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
            axios.delete(base_url + '/paket/'+ id, config)
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
