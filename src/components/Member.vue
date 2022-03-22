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
                                    <td>ID Member</td>
                                    <td>Nama</td>
                                    <td>jenis kelamin</td>
                                    <td>No Telpone</td>
                                    <td>Alamat</td>
                                    <td>Aksi</td>
                                </tr>
                                <tr v-for="mber in member" :key="mber">
                                    <td>{{ mber.id_member }}</td>
                                    <td>{{ mber.name }}</td>
                                    <td>
                                    <span v-if="mber.jenis_kelamin === 'l'" class="badge bg-success">Laki-laki</span>
                                    <span v-if="mber.jenis_kelamin === 'p'" class="badge bg-danger">Perempuan</span>
                                    </td>
                                    <td>{{ mber.tlp }}</td>
                                    <td>{{ mber.alamat }}</td>
                                    <td>
                                        <a v-b-modal.modal_member href="#" class="btn btn-success" @click="Edit(mber)">Ubah</a>
                                        <a href="#" class="btn btn-danger" @click="Delete(mber.id_member)">Hapus</a>
                                    </td>
                                </tr>
                    
                </table>
              </div>
            </div>
          </div>
        </div>
      </div>
        </main>

        <b-modal id="modal_member" ref="modal" title="Form Member" size="md" @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="name" placeholder="Enter your first name" id="inputNama" class="form-control" type="text"/>
                    <label for="inputNama">Nama</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="tlp" placeholder="Enter your first name" v-modal="tlp" id="inputNama" class="form-control" type="text"/>
                    <label for="inputTlp">Telpone</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    
                    <select id="inputjenis" class="form-control" v-model="jenis_kelamin">
                    <option value="">--Pilih jenis kelamin--</option>
                    <option value="l">Laki laki</option>
                    <option value="p">Perempuan</option>
                    </select>
                    <label for="inputJenis" >Jenis Kelamin</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="alamat" placeholder="Enter your first name" v-modal="alamat" id="inputNama" class="form-control" type="text"/>
                    <label for="inputAlamat">Alamat</label>
                </div>
            </form>
        </b-modal>

    </div>
</template>
<script>
module.exports = {
    data: function(){
        return{
            name:"",
            jenis_kelamin : "",
            tlp : "",
            alamat : "",
            member :[],
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
        axios.get(base_url + '/member', config)
        .then(response=>{
                console.log(response);
            if(response.data.success == true){
                this.member = response.data.data.member;
        }  
    })
},
Add: function(){
    this.action = "insert";
    this.id_member = "";
    this.name = "";
    this.jenis_kelamin = "";
    this.tlp = "";
    this.alamat = "";
},
Edit: function(item){
    this.action = "update";
    this.id_member = item.id_member;
    this.name = item.name;
    this.jenis_kelamin = item.jenis_kelamin;
    this.tlp = item.tlp;
    this.alamat = item.alamat;
},

Save: function(){
    let config = {
        headers : {
            "Authorization":"Bearer "+this.$cookies.get('Authorization')
        }
    }

    let form = {
        "name" : this.name,
        "alamat" : this.alamat,
        "jenis_kelamin": this.jenis_kelamin,
        "tlp": this.tlp,
    }
    if(this.action == "insert"){
        axios.post(base_url + '/member', form, config)
        .then( response => {
            Swal.fire(response.data.message)
    })
    }else{
        axios.put(base_url + '/member/'+this.id_member, form, config)
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
            axios.delete(base_url + '/member/'+ id, config)
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