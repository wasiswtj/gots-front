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
import axios from 'axios';

export default {
  components: { TabOne, TabTwo, TabThree, TabFour },
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
        nama_penaksir: '',
        nama_outlet: ''
      }],
    }
  },
  beforeMount() {
    new Promise((resolve, reject) => {//10.83.9.142 SB, 192.168.100.133 LT3, 192.168.100.47 LT2, 182.28.131.48 GIG, 10.30.40.112 PGD
      this.$http({url: 'http://192.168.43.13:9000/api/status_pengajuan', data: '', method: 'GET', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        this.dataPengajuan = resp.data.data
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
