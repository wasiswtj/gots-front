<template>
  <nb-container>
    <nb-header :style="{ marginTop: 20}">
      <nb-button transparent :style="{marginLeft:-20}" :onPress="() => this.props.navigation.goBack()">
        <nb-icon name="arrow-back" />
      </nb-button>  
      <nb-body>
        <nb-title :style="{marginLeft:5}">Riwayat Anda</nb-title>
      </nb-body>
    </nb-header>
    <nb-content>
      <nb-card>
        <nb-card-item header>
          <nb-text>Proses Pencairan Dana</nb-text>
        </nb-card-item>
        <nb-card-item>
          <nb-body>
            <nb-text>No Pengajuan: {{statusTransaksi.no_transaksi}}</nb-text>
            <nb-text></nb-text>
            <nb-text>INFORMASI NASABAH</nb-text>
            <nb-text>No CIF              : {{statusTransaksi.no_cif}}</nb-text>
            <nb-text>Nama               : {{statusTransaksi.nama_nasabah}}</nb-text>
            <nb-text>Alamat             : {{statusTransaksi.alamat_nasabah}}</nb-text>
            <nb-text></nb-text>
            <nb-text>INFORMASI PENAKSIR</nb-text>
            <nb-text>NIK                     :  {{statusTransaksi.no_nik_penaksir}}</nb-text>
            <nb-text>nama                   :  {{statusTransaksi.nama_penaksir}}</nb-text>
            <nb-text>No Handphone       :  {{statusTransaksi.no_handphone_penaksir}}</nb-text>
            <nb-text>Cabang                :  {{statusTransaksi.nama_outlet}}</nb-text>
            <nb-text>No Telp Cabang     :  {{statusTransaksi.no_telepon_cabang}}</nb-text>
            <nb-text></nb-text>
            <nb-text>INFORMASI BARANG</nb-text>
            <nb-text>Jenis Perhiasan    '': {{statusTransaksi.jenis_perhiasan}}</nb-text>
            <nb-text>Kadar Emas          : {{statusTransaksi.kadar}}</nb-text>   
            <nb-text>Berat Kotor          : {{statusTransaksi.berat_kotor}}</nb-text>
            <nb-text>Berat Bersih         : {{statusTransaksi.berat_bersih}}</nb-text>
            <nb-text>Deskripsi Barang   : {{statusTransaksi.keterangan_barang}}</nb-text>
            <nb-text></nb-text>
            <nb-text>INFORMASI PINJAMAN</nb-text>
            <nb-text>Jumlah Pinjaman          : {{statusTransaksi.jumlah_pinjaman}}</nb-text>
            <nb-text>Biaya Administrasi       : {{statusTransaksi.biaya_administrasi}}</nb-text>
            <nb-text>Biaya Asuransi             : {{statusTransaksi.biaya_asuransi}}</nb-text>
            <nb-text>Biaya Jasa Perjalanan  : {{statusTransaksi.biaya_perjalanan}}</nb-text>
            <nb-text>-----------------------------------------------------------</nb-text>
            <nb-text>Total Diterima         : {{statusTransaksi.jumlah_diterima}}</nb-text>
            
          </nb-body>            
        </nb-card-item>
      </nb-card>
    </nb-content>
  </nb-container>
</template>
<script>
export default {
  data() {
    return{
    statusTransaksi:{
      id:'',
      no_transaksi:'',
      no_cif:'',
      nama_nasabah:'',
      alamat_nasabah:'',
      jenis_perhiasan:'',
      kadar:'',
      berat_kotor:'',
      berat_bersih:'',
      keterangan_barang:'',
      jumlah_pinjaman:'',
      biaya_administrasi:'',
      biaya_asuransi:'',
      biaya_perjalanan:'',
      status_transaksi:'',
      tanggal_gadai:'',
      jumlah_diterima:'',
      no_nik_penaksir:'',
      nama_penaksir:'',
      no_handphone_penaksir:'',
      nama_outlet:'',
      no_telepon_cabang:''

    }
  }
},
beforeMount() {
    new Promise((resolve, reject) => {//10.83.9.142 SB, 192.168.100.133 LT3, 192.168.100.47 LT2, 182.28.131.48 GIG, 10.30.40.112 PGD
      this.$http({url: 'http://192.168.43.76:9090/api/transaksi_berjalan/1', data: '', method: 'GET', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        this.statusTransaksi=resp.data.data
      resolve(resp)
      })
      .catch(err => {
        // commit('auth_error', err)
        console.log(err)
        reject(err)
      })
    })
  }}
</script>