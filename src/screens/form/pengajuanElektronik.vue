<template>
  <nb-container :style="{ backgroundColor: '#fff' }">
    <nb-header :style="{ marginTop: 20}">
      <nb-left>
        <nb-button transparent :onPress="() => this.props.navigation.goBack()">
          <nb-icon name="arrow-back" />
        </nb-button>
      </nb-left>
      <nb-body>
        <nb-title>Pengajuan Gadai Elektronik</nb-title>
      </nb-body>
    </nb-header>

    <nb-content padder>
      <nb-form>
        <nb-label >Jenis Elektronik</nb-label> 
        <nb-picker
          note
          mode="dropdown"
          :style="{ width: 350 }"
          :selectedValue="jenisElektronik"
          :onValueChange="onValueChange"
      
        >
          <item label="Handphone" value='0' />
          <item label="Laptop/PC" value='1' />
          <item label="Kamera" value='2' />
          <item label="Televisi" value='3'/>
          <item label="Sound System / Alat Musik Listrik" value='4'/>
          <item label="Kulkas" value='5'/>
          <item label="Elektronik Lainnya" value='6'/>
        </nb-picker>
        <nb-item stackedLabel>
          <nb-label >Merk</nb-label>
          <nb-input v-model="merk"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Tipe</nb-label>
          <nb-input v-model="tipe"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Tahun Pembelian</nb-label>
          <nb-input v-model="tahunBeli"/>
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
import React from "react";
import { Toast,Picker } from "native-base";
import * as ImagePicker from "expo-image-picker";

export default {
  components:{Item:Picker.Item},
  props: {
    navigation: {
      type: Object
    }
  },
  data() {
    return {
      jenisElektronik: '',
      merk: '',
      tipe: '',
      tahunBeli: '',
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
    onValueChange: function(value) {
      this.jenisElektronik = value;
    },
    submitData: function () {
      var $data = {
        no_pengajuan: "9992020011800001",
        jenis_Elektronik: this.jenisElektronik,
        merk: this.merk,
        tipe: this.tipe,
        tahunBeli: this.tahunBeli,
        keterangan_barang: this.keteranganBarang,
        titik_gadai: "-6.191365, 106.781534",
        foto_perhiasan: this.fotoPerhiasan,
      }
      //10.83.9.142 SB, 192.168.100.133 LT3, 192.168.100.47 LT2, 182.28.131.48 GIG, 10.30.40.112 PGD
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
