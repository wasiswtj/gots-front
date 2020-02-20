<template>
  <nb-container :style="{ backgroundColor: '#fff' }">
    <nb-header>
      <nb-left>
        <nb-button transparent :onPress="() => this.props.navigation.goBack()">
          <nb-icon name="arrow-back" />
        </nb-button>
      </nb-left>
      <nb-body>
        <nb-title>Pengajuan Gadai Perhiasan</nb-title>
      </nb-body>
      <nb-right />
    </nb-header>

    <nb-content padder>
      <nb-form>
        <nb-item stackedLabel>
          <nb-label >Jenis Perhiasan</nb-label>
          <nb-input v-model="jenisPerhiasan"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Kadar</nb-label>
          <nb-input v-model="kadar"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Berat Kotor</nb-label>
          <nb-input v-model="beratKotor"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Berat Bersih</nb-label>
          <nb-input v-model="beratBersih"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Alamat</nb-label>
          <nb-input v-model="titikGadai"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Deskripsi Barang</nb-label>
          <nb-input v-model="keteranganBarang"/>
        </nb-item>
      </nb-form>
      <nb-button :onPress="submitData" block :style="{ margin: 15, marginTop: 50 }">
        <nb-text>Sign In</nb-text>
      </nb-button>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform } from "react-native";
import { store } from "../../boot/setup.vue"
import { Toast } from "native-base";

export default {
  props: {
    navigation: {
      type: Object
    }
  },
  data() {
    return {
      jenisPerhiasan: '',
      kadar: '',
      beratKotor: '',
      beratBersih: '',
      alamat: '',
      keteranganBarang:'',
      titikGadai: '',
      fotoPerhiasan: ''
    };
  },
  created() {
    // console.log(store.state.token)
    // AsyncStorage.getItem('email').then((val) => {
    //   if (val) {
    //     this.loaded = true
    //     this.navigation.navigate('Home')
    //     store.dispatch('SET_USER', {userObj: {email: val}})
    //   } else {
    //     this.loaded = true
    //   }
    // })
  },
  methods: {
    submitData: function () {
      var $data = {
        no_pengajuan: "9992020011800001",
        jenis_perhiasan: this.jenisPerhiasan,
        kadar: this.kadar,
        berat_kotor: this.beratKotor,
        berat_bersih: this.beratBersih,
        keterangan_barang: this.keteranganBarang,
        titik_gadai: "-6.191365, 106.781534",
        foto_perhiasan: "Base 64",
      }
      
      this.$http({url: 'http://192.168.43.13:9000/api/pengajuan', data: $data, method: 'POST', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        
        console.log(resp)
      })
      .catch(err => {
        // commit('auth_error', err)
        this.showIncompleteData()
        console.log(err)
      })
    },
    showIncompleteData() {
      Toast.show({
        text: "Harap periksa kembali data pengajuan anda!",
        buttonText: "Okay",
        type: "danger"
      });
    }
  }
};
</script>
