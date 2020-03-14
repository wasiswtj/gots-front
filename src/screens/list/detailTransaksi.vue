<template>
  <nb-container>
    <nb-header :style="{ marginTop: 20}">
      <nb-button transparent :style="{marginLeft:-20}" :onPress="() => this.props.navigation.goBack()">
        <nb-icon name="arrow-back" />
      </nb-button>  
      <nb-body>
        <nb-title :style="{marginLeft:5}">Transaksi Anda</nb-title>
      </nb-body>
    </nb-header>
    <nb-content>
      <nb-list>
        <nb-list-item
         button
        :onPress="() => this.props.navigation.navigate(StatusTransaksiAnda)">
          <nb-left>
            <nb-text>Detail Transaksi</nb-text>
          </nb-left>
        </nb-list-item>
        <nb-list-item 
         button
        :onPress="sbkLink" :style="{marginRight:-15}">
          <nb-left>
            <nb-text>Surat Bukti Kredit</nb-text>
          </nb-left>
        </nb-list-item>
        <nb-list-item 
         button
        :onPress="strukLink" :style="{marginRight:-15}" >
          <nb-left>
            <nb-text>Struk</nb-text>
          </nb-left>
        </nb-list-item>
      </nb-list>
    </nb-content>
  </nb-container>
</template>

<script>
import * as WebBrowser from 'expo-web-browser';
export default {
    data() {
    return{
        StatusTransaksiAnda:"StatusTransaksiAnda",
        detailTransaksi:{
            id:'',
            uri_sbk:'',
            uri_struk:'',
            status_transaksi:'proses',
            tanggal_gadai:'kkk',
    }
  }

},
methods:{
    async sbkLink() {
      let result = await WebBrowser.openBrowserAsync(this.detailTransaksi.uri_sbk);
    },
    async strukLink() {
      let result = await WebBrowser.openBrowserAsync(this.detailTransaksi.uri_struk);
    }
},
beforeMount() {
    new Promise((resolve, reject) => {//10.83.9.142 SB, 192.168.100.133 LT3, 192.168.100.47 LT2, 182.28.131.48 GIG, 10.30.40.112 PGD
      this.$http({url: 'http://192.168.43.76:9090/api/transaksi_berjalan/1', data: '', method: 'GET', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        this.detailTransaksi=resp.data.data
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

</style>
<style>
  .bg-style {
    background-color: #cde1f9;
  }
</style>