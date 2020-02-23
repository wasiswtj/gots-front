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

      <nb-card v-if="fotoPerhiasan!=''" :style="{ flex:1, marginTop: 15 }">
        <nb-card-item cardBody>
          <image class="card-item-image"
            :style="{height: 300, flex:1}"
            :source="{uri: 'data:image/jpeg;base64,' + fotoPerhiasan}"
          />
        </nb-card-item>
      </nb-card>

      <nb-button :onPress="_pickImage"
        dark bordered rounded
        :style="{ marginBottom: 15, marginTop: 15, flex: 1}"
      >
        <nb-icon active name="camera" />
      </nb-button>

      <nb-button :onPress="submitData" rounded success :style="{ flex:1, marginBottom: 15 }">
        <nb-text>Ajukan Gadai</nb-text>
      </nb-button>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform, Alert } from "react-native";
import { Toast } from "native-base";
import * as ImagePicker from "expo-image-picker";

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
  mounted() {
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
        foto_perhiasan: this.fotoPerhiasan,
      }
      
      this.$http({url: 'http://192.168.43.13:9000/api/pengajuan', data: $data, method: 'POST', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        console.log(resp)
        Alert.alert(
          'Informasi',
          'Pengajuan Gadai Berhasil',
          [
            {text: 'Menuju Pengajuan Anda', onPress: () => this.navigation.navigate("StatusPengajuan")},
          ],
          { cancelable: false }
        )
      })
      .catch(err => {     
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
    },
    async _pickImage(){
      let response = await ImagePicker.launchCameraAsync({
        base64: true,
        quality: 0.5
      });

      this.fotoPerhiasan = response.base64
    }
  }
};
</script>
