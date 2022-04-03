<template>
    <div>
        <main>
                    <div class="container-fluid py-4">
                        <div class="row">
                            <div class="col-12">
                            <div class="card mb-4">
                                <div class="card-header pb-0">
                                <h1>Transaksi</h1>
                                </div>
                            <div class="card-body px-0 pt-0 pb-2">
                            <div class="table-responsive p-4">
                                
                                    <router-link to="/input-transaksi" class="btn btn-sm btn-info btn-icon" v-if="role !== 'owner'">
                                    <div>
                                    <i class="fa-solid fa-cart-plus"></i>
                                    </div>
                                    </router-link>

                                    <form>
                        
                                        <div class="col-lg-2 col-sm-6">
                                            <label for="PilihTahun">tahun</label>
                                            <b-form-select class="form-control" @change="getData($event)" v-model="tahun" :options="list_years"></b-form-select>
                                            
                                        </div>
                                        <div class="col-lg-2 col-md-6">
                                            <label for="PilihBulan">bulan</label>
                                            <b-form-select class="form-control" @change="getData($event)" v-model="bulan" :options="list_months"></b-form-select>
                                        </div>
                                        
                                    </form>
                                    <table class="table align-items-center mb-0">
                                        <tr>
                                            <td>ID Transaksi</td>
                                            <td>Nama Member</td>
                                            <td>Tanggal</td>
                                            <td>Status</td>
                                            <td>Status Bayar</td>
                                            <td>Tanggal Bayar</td>
                                            <td>Kasir</td>
                                            <td>Total</td>
                                            <td>Aksi</td>
                                        </tr>
                                        <tr v-for="(saksi, index) in transaksi" :key="index">
                                            <td>{{ index+1 }}</td>
                                            <td>{{ saksi.nama_member }}</td>
                                            <td>{{ saksi.tgl }}</td>
                                            <td>
                                                <span v-if="role !== 'owner'">
                                                <select class="form-control" @change="changeStatus(saksi.id_transaksi, $event)">
                                                    <option value="baru"    :selected="saksi.status === 'baru'">Baru</option>
                                                    <option value="proses"  :selected="saksi.status === 'proses'">Proses</option>
                                                    <option value="selesai" :selected="saksi.status === 'selesai'">Selesai</option>
                                                    <option value="diambil" :selected="saksi.status === 'diambil'">Diambil</option>
                                                </select>
                                                </span>
                                                <span v-if="role === 'owner'">
                                                    {{ saksi.status }}
                                                </span>
                                            </td>
                                            <td>
                                                <span v-if="role !== 'owner'">
                                                <select class="form-control" @change="changeBayar(saksi.id_transaksi, $event)">
                                                    <option value="dibayar"    :selected="saksi.dibayar === 'dibayar'">Dibayar</option>
                                                    <option value="belum_dibayar"  :selected="saksi.dibayar === 'belum_dibayar'">Belum Dibayar</option>
                                                </select>
                                                </span>
                                                <span v-if="role === 'owner'">
                                                    {{ saksi.dibayar }}
                                                </span>
                                            </td>
                                            <td>{{ saksi.tgl_bayar }}</td>
                                            <td>{{ saksi.kasir }}</td>
                                            <td>{{ saksi.total }}</td>
                                            <td>
                                                <a v-b-modal.modal_detail href="#" class="btn btn-warning" @click="detail(saksi.detail_transaksi, saksi.total)">
                                                    <i class="fa-solid fa-circle-info"></i>
                                                </a>
                                            </td>
                                        </tr>
                                        </table>

                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        </main>
        <b-modal id="modal_detail" ref="modal" title="Detail Transaksi" size="md" hide-footer="true">
            <a class="btn btn-success" @click="cetak()">Cetak</a>
            <div id="print">
            <table class="table">
                                <tr>
                                    <td>Kasir :</td>
                                    <td>{{ name }}</td>
                                </tr>
                                <tr>
                                    <td>#</td>
                                    <td>Jenis</td>
                                    <td>Berat</td>
                                    <td>Sub total</td>
                                </tr>
                                <tr v-for="(det, index) in detail_transaksi" :key="index" >
                                    <td>{{ index+1 }}</td>
                                    <td>{{ det.jenis }}</td>
                                    <td>{{ det.berat }}</td>
                                    <td>{{ det.sub_total }}</td>
                                </tr>
                            </table>
                            <div class="text-right"><h4>Total: Rp{{ total }}</h4></div>
                            </div>
        </b-modal>
    </div>
    

</template>
<script> 
module.exports ={
    data: function(){
        return{
            id_transaksi:"",
            nama_member:"",
            tgl:"",
            status:"",
            dibayar:"",
            tgl_bayar:"",
            kasir:"",
            total:"",
            tahun:"",
            name:"",
            bulan:"",
            tgl:"",
            role:"",
            action:"",
            total:"",
            role:"",
            transaksi:[],
            detail_transaksi:[],
            list_years: [2020, 2021, 2022, 2023],
            list_months: [1,2,3,4,5,6,7,8,9,10,11,12]
        }
    },
    methods:{
        getData: function(){
            let config = { 
                headers: {
                "Authorization" : "Bearer "+ this.$cookies.get('Authorization')
            }
        };
            let form = {
                "tahun": this.tahun,
                "bulan": this.bulan,
                "tgl"  : this.tgl 
            }
        axios.post(base_url + '/transaksi/report', form, config)
        .then(response =>{
            this.transaksi = response.data.data;
            
        })
        .catch(error => {
            console.log(error);
        });
        },
        getRole: function(){
            let config = {
            headers: {
                "Authorization":"Bearer "+this.$cookies.get('Authorization')
            }
        }
        axios.get(base_url + '/login/check', config)
        .then(response=>{
            if(response.data.success == true){
                this.username = response.data.data.username;
                this.name = response.data.data.name;
                this.role = response = response.data.data.role;
        }  
    })

        },
        changeStatus: function(id_transaksi, event){
            let conf = { 
                headers: { 
                    "Authorization" : 'Bearer ' +this.$cookies.get('Authorization') 
                } 
            }
            let form = {
                "id_transaksi": id_transaksi,
                "status": event.target.value
            }
            axios.put(base_url + "/transaksi/status", form, conf)
        .then(response => {
            this.getData();
            Swal.fire(response.data.message)
        })
        .catch(error => {
            console.log(error);
        });
        },
        changeBayar: function(id_transaksi, event){
            let conf = { 
                headers: { 
                    "Authorization" : 'Bearer ' +this.$cookies.get('Authorization') 
                } 
            }
            let form = {
                "id_transaksi": id_transaksi,
                "dibayar": event.target.value
            }
        axios.put(base_url + "/transaksi/bayar", form, conf)
        .then(response => {
            this.getData();
            Swal.fire(response.data.message)
        })
        .catch(error => {
            console.log(error);
        });
        },
        detail: function(detail_transaksi, total){
            this.total = total;
            this.detail_transaksi = detail_transaksi;
        },
        cetak: function(){
      const prtHtml = document.getElementById('print').innerHTML;


      console.log(prtHtml);
      let stylesHtml = '';
      for (const node of [...document.querySelectorAll('link[rel="stylesheet"], style')]) {
        stylesHtml += node.outerHTML;
      }
      const WinPrint = window.open('', '', 'left=0,top=0,width=800,height=900,toolbar=0,scrollbars=0,status=0');
      WinPrint.document.write(`<!DOCTYPE html>
      <html>
        <head>

        <link rel="stylesheet" href="src/assets/css/bootstrap.min.css">
          
        </head>
        <body>
          ${prtHtml}
        </body>
      </html>`);
      WinPrint.document.close();
      WinPrint.focus();
      WinPrint.print();
      WinPrint.close();
    }

    },
    mounted(){
        this.getData();
        this.getRole();
    }

}
</script>