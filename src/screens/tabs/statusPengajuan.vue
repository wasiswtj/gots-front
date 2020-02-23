<template>
  <nb-container>
    <nb-header hasTabs>
      <nb-left>
        <nb-button transparent :onPress="() => this.props.navigation.navigate('Home')">
          <nb-icon name="arrow-back" />
        </nb-button>
      </nb-left>
      <nb-body>
        <nb-title> Status Pengajuan</nb-title>
      </nb-body>
    </nb-header>

    <nb-content v-if="loadingScreen">
        <nb-spinner color="blue" :style="{marginTop: 50}"/>
    </nb-content>

    <nb-tabs :initialPage=tabActive :renderTabBar="getScollableTabComp" v-if="renderComponent">
      <nb-tab heading="Menunggu Konfirmasi">
        <tab-one v-if="tabActive==0" v-bind:dataPengajuan="dataPengajuan"/>
      </nb-tab>
      <nb-tab heading="Dikonfirmasi">
        <tab-two v-if="tabActive==1" v-bind:dataPengajuan="dataPengajuan"/>
      </nb-tab>
      <nb-tab heading="Menuju Lokasi">
        <tab-three v-if="tabActive==2" v-bind:dataPengajuan="dataPengajuan"/>
      </nb-tab>
      <nb-tab heading="Penaksir Tiba">
        <tab-four v-if="tabActive==3" v-bind:dataPengajuan="dataPengajuan"/>
      </nb-tab>
      <nb-tab heading="Disetujui">
        <tab-five v-if="tabActive==4" v-bind:dataPengajuan="dataPengajuan"/>
      </nb-tab>
    </nb-tabs>
  </nb-container>
</template>

<script>
import React from "react";
import { ScrollableTab } from "native-base";
import TabOne from "./components/tabOne";
import TabTwo from "./components/tabTwo";
import TabThree from "./components/tabThree";
import TabFour from "./components/tabFour";
import TabFive from "./components/tabFive";
import axios from 'axios';

export default {
  components: { TabOne, TabTwo, TabThree, TabFour, TabFive },
  methods: {
    getScollableTabComp() {
      return <ScrollableTab />;
    },
  },
  data() {
    return {
      tabActive: 0,
      renderComponent: false,
      loadingScreen: true,
      dataPengajuan : [{ 
        noPengajuan: '',
        jenisPerhiasan: '',
        no_pengajuan: '',
        no_cif: '',
        jenis_perhiasan: '',
        kadar: '',
        berat_kotor: '',
        berat_bersih: '',
        perkiraan_harga: '',
        harga_taksir: '',
        id_hps: '',
        keterangan_barang: '',
        titik_gadai: '',
        provinsi: '',
        kabupaten: '',
        kecamatan: '',
        kelurahan: '',
        foto_perhiasan: '',
        nama_penaksir: ''
      }],
    }
  },
  beforeMount() {
    new Promise((resolve, reject) => {
      this.$http({url: 'http://192.168.43.13:9000/api/status_pengajuan', data: '', method: 'GET', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        this.dataPengajuan.no_pengajuan= resp.data.data.no_pengajuan
        this.dataPengajuan.no_cif= resp.data.data.no_cif
        this.dataPengajuan.jenis_perhiasan= resp.data.data.jenis_perhiasan
        this.dataPengajuan.kadar= resp.data.data.kadar
        this.dataPengajuan.berat_kotor= resp.data.data.berat_kotor
        this.dataPengajuan.berat_bersih= resp.data.data.berat_bersih
        this.dataPengajuan.perkiraan_harga= resp.data.data.perkiraan_harga
        this.dataPengajuan.harga_taksir= resp.data.data.harga_taksir
        this.dataPengajuan.id_hps= resp.data.data.id_hps
        this.dataPengajuan.keterangan_barang= resp.data.data.keterangan_barang
        this.dataPengajuan.titik_gadai= resp.data.data.titik_gadai
        this.dataPengajuan.provinsi= resp.data.data.provinsi
        this.dataPengajuan.kabupaten= resp.data.data.kabupaten
        this.dataPengajuan.kecamatan= resp.data.data.kecamatan
        this.dataPengajuan.kelurahan= resp.data.data.kelurahan
        this.dataPengajuan.foto_perhiasan= resp.data.data.foto_perhiasan
        this.dataPengajuan.nama_penaksir = resp.data.data.nama_penaksir

        this.tabActive = Number(resp.data.data.id_status)-1
        this.renderComponent = true
        this.loadingScreen = false
        resolve(resp)
      })
      .catch(err => {
        // commit('auth_error', err)
        console.log(err)
        reject(err)
      })
    })
  }
};
</script>
