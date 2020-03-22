<template>
  <nb-container>
    <nb-header >
      <nb-button transparent :style="{marginLeft:-20}" :onPress="() => this.props.navigation.goBack()">
        <nb-icon name="arrow-back" />
      </nb-button>  
      <nb-body>
        <nb-title :style="{marginLeft:5}">Transaksi Anda</nb-title>
      </nb-body>
    </nb-header>
    <nb-content>
      <nb-list >
        <nb-list-item 
        v-for="listItem in dataTransaksi"
        :key="listItem.id"
        button
        :onPress="() => this.props.navigation.navigate(DetailTransaksi)"
        >
          <nb-left>
            <nb-body>
              <nb-text :style="{fontSize: 18, color:'blue'}" >No Transaksi {{dataTransaksi[0].no_transaksi}}</nb-text>
              <nb-text :style="{fontSize: 16}">Pinjaman: {{dataTransaksi[0].jumlah_pinjaman}}</nb-text>
              <nb-text :style="{fontSize: 16}">Status: {{dataTransaksi[0].status_transaksi}}</nb-text>
              <nb-text :style="{fontSize: 13, color:'blue'}">{{dataTransaksi[0].tanggal_gadai}}</nb-text>
              </nb-body>
            </nb-left>
        </nb-list-item>
      </nb-list>
    </nb-content>
  </nb-container>
</template>
<script>
export default {
  // props:[this.dataTransaksi[0].id],
  data() {
  return{
    DetailTransaksi:"DetailTransaksi",
    dataTransaksi:[{
      id:'',
      no_transaksi:'',
      jumlah_pinjaman:'',
      status_transaksi:'',
      tanggal_gadai:'',
    }]
  }
},
beforeMount() {
    new Promise((resolve, reject) => {//10.83.9.142 SB, 192.168.100.133 LT3, 192.168.100.47 LT2, 182.28.131.48 GIG, 10.30.40.112 PGD
      this.$http({url: 'http://192.168.43.13:9000/api/transaksi_berjalan/'+this.dataTransaksi[0].id, data: '', method: 'GET', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        this.dataTransaksi=resp.data.data
      resolve(resp)
      })
      .catch(err => {
        // commit('auth_error', err)
        console.log(err)
        reject(err)
      })
    })
  }  
}

</script>
<style>
  .bg-style {
    background-color: #cde1f9;
  }
</style>