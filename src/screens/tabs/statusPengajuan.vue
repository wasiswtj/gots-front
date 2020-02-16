<template>
  <nb-container v-if="renderComponent">
    <nb-header hasTabs>
      <nb-left>
        <nb-button transparent :onPress="() => this.props.navigation.goBack()">
          <nb-icon name="arrow-back" />
        </nb-button>
      </nb-left>
      <nb-body>
        <nb-title> Status Pengajuan</nb-title>
      </nb-body>
    </nb-header>

    <nb-tabs :initialPage=tabActive :renderTabBar="getScollableTabComp">
      <nb-tab heading="Menunggu Konfirmasi">
        <tab-one />
      </nb-tab>
      <nb-tab heading="Dikonfirmasi">
        <tab-two />
      </nb-tab>
      <nb-tab heading="Menuju Lokasi">
        <tab-three />
      </nb-tab>
      <nb-tab heading="Penaksir Tiba">
        <tab-four />
      </nb-tab>
      <nb-tab heading="Disetujui">
        <tab-five />
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
import { store } from "../../boot/setup.vue"
import axios from 'axios'

export default {
  components: { TabOne, TabTwo, TabThree, TabFour, TabFive },
  methods: {
    getScollableTabComp() {
      return <ScrollableTab />;
    },
    forceRerender() {
      // Remove my-component from the DOM
      this.renderComponent = false;
      
      this.$nextTick(() => {
        // Add the component back in
        this.renderComponent = true;
      });
    }
  },
  data() {
    return {
      tabActive: null,
      noPengajuan: '',
      renderComponent: true,
    }
  },
  beforeMount() {
    new Promise((resolve, reject) => {
      this.$http({url: 'http://10.83.9.141:9000/api/status_pengajuan', data: '', method: 'GET', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        // const token = resp.data.token
        // axios.defaults.headers.common['Authorization'] = token
        // commit('auth_success', token)
        this.tabActive = Number(resp.data.data.id_status)
        console.log(this.tabActive)
        resolve(resp)
      })
      .catch(err => {
        // commit('auth_error', err)
        console.log(err)
        reject(err)
      })
    })
    this.forceRerender()
  }
};
</script>
