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
        <nb-text :style="{ fontSize:15, marginLeft: 13, marginTop: 10, color: '#575757' }">Jenis Perhiasan</nb-text> 
        <nb-picker
          mode="dropdown"
          :style="{ width: 340, marginLeft:8,color:'black' }"
          :selectedValue="jenisPerhiasan"
          :onValueChange="onValueChangePerhiasan"
        >
          <item label="Logam Mulia" value="Logam Mulia" />
          <item label="Cincin" value="Cincin" />
          <item label="Gelang" value="Gelang" />
          <item label="Anting / Giwang" value="Anting"/>
          <item label="Kalung" value="Kalung"/>
          <item label="Liontin" value="Liontin"/>
        </nb-picker>
        <nb-text :style="{ fontSize:15, marginLeft: 13, marginTop: 10, color: '#575757' }">Kadar</nb-text> 
        <nb-picker
          mode="dropdown"
          :style="{ width: 340, marginLeft:8,color:'black' }"
          :selectedValue="kadar"
          :onValueChange="onValueChangeKadar"
        >
          <item label="16 karat" value='16'/>
          <item label="17 karat" value='17'/>
          <item label="18 karat" value='18'/>
          <item label="19 karat" value='19'/>
          <item label="20 karat" value='20'/>
          <item label="21 karat" value='21'/>
          <item label="22 karat" value='22'/>
          <item label="23 karat" value='23'/>
          <item label="24 karat" value='24'/>
        </nb-picker>
        <!-- <nb-item stackedLabel>
          <nb-label >Jenis Perhiasan</nb-label>
          <nb-input v-model="jenisPerhiasan"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Kadar</nb-label>
          <nb-input v-model="kadar"/>
        </nb-item> -->
        <nb-item stackedLabel>
          <nb-label >Berat Kotor (gram)</nb-label>
          <nb-input v-model="beratKotor" keyboardType="numeric"/>
        </nb-item>
        <nb-item stackedLabel>
          <nb-label >Berat Bersih (gram)</nb-label>
          <nb-input v-model="beratBersih" keyboardType="numeric"/>
        </nb-item>

        <nb-card :style="{ marginTop: 15, marginLeft: 10 }" v-if="perkiraanHarga!=''">
          <nb-card-item :style="{backgroundColor: '#55BB5E'}">
            <nb-left>
              <nb-icon name="wallet" :style="{color: '#fff'}"></nb-icon>
              <nb-text :style="{color: '#fff'}">Rp {{perkiraanHarga}} (Estimasi)</nb-text>
            </nb-left>
          </nb-card-item>
        </nb-card>

        <nb-text :style="{ fontSize: 10, marginBottom: 15, marginLeft: 10 }" v-if="perkiraanHarga!=''">*Perkiraan harga diatas belum termasuk Biaya Administrasi, Asuransi, dan Jasa Perjalanan.</nb-text>

        <nb-item stackedLabel>
          <nb-label >Alamat</nb-label>
          <nb-input v-model="alamat"/>
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
        <nb-icon v-if="fotoPerhiasan==''" active name="camera"/>
        <nb-text v-if="fotoPerhiasan!=''"> Ulangi</nb-text>
      </nb-button>

      <nb-button :onPress="submitData" rounded success :style="{ flex:1, marginBottom: 15 }">
        <nb-text>Ajukan Gadai</nb-text>
      </nb-button>

    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform, Alert } from "react-native";
import { Toast, Picker } from "native-base";
import { Constants, MapView } from 'expo';
import * as Permissions from 'expo-permissions';
import * as Location from 'expo-location';
import * as ImagePicker from "expo-image-picker";
import _ from "lodash";

export default {
  components:{ Item:Picker.Item },
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
      fotoPerhiasan: '',
      perkiraanHarga: '',
      userLocation: {coords: { latitude: null, longitude: null}},
    };
  },
  created() {
    this.debouncedApiCall = _.debounce(this.getPerkiraanHarga, 500)
  },
  mounted() {
    this._getLocationAsync()
  },
  methods: {
    submitData: function () {
      var $data = {
        jenis_perhiasan: this.jenisPerhiasan,
        kadar: this.kadar,
        berat_kotor: this.beratKotor,
        berat_bersih: this.beratBersih,
        keterangan_barang: this.keteranganBarang,
        alamat: this.alamat,
        latitude: this.userLocation.coords.latitude,
        longitude: this.userLocation.coords.longitude,
        foto_perhiasan: this.fotoPerhiasan,
      }
      
      this.$http({url: 'http://192.168.43.13:9000/api/pengajuan', data: $data, method: 'POST', headers: {'Content-Type': 'application/json' }})
      .then(resp => {
        console.log(resp)
        if (resp.data.status == 'error') {
          this.showBackendMessage(resp.data.message)
          return
        }
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
        type: "danger",
        duration: 4000
      });
    },
    showBackendMessage(message) {
      Toast.show({
        text: message,
        buttonText: "Okay",
        type: "danger",
        duration: 4000
      });
    },
    getPerkiraanHarga(){
      var $data = {
        jenis_perhiasan: this.jenisPerhiasan,
        kadar: this.kadar,
        berat_kotor: this.beratKotor,
        berat_bersih: this.beratBersih,
      }

      if (this.beratBersih != '') {
        this.$http({url: 'http://192.168.43.13:9000/api/perkiraan_harga_perhiasan', params: $data, method: 'GET', headers: {'Content-Type': 'application/json' }})
        .then(resp => {
          this.perkiraanHarga = resp.data.data.perkiraan_harga
          console.log(resp.data)
        })
        .catch(err => {
          console.log(err)
        })
        return
      }
      
      this.perkiraanHarga = ''
    },
    async _pickImage(){
      let response = await ImagePicker.launchCameraAsync({
        base64: true,
        quality: 0.5
      });

      if (response.cancelled == true) {
        return
      }

      this.fotoPerhiasan = response.base64
    },
    async _getLocationAsync () {
      let { status } = await Permissions.askAsync(Permissions.LOCATION);
      if (status !== 'granted') {
        this.setState({
          userLocation: 'Permission to access location was denied',
          location,
        });
      }

      let location = await Location.getCurrentPositionAsync({});
      this.userLocation = location;
      console.log (this.userLocation)
    },
    onValueChangePerhiasan: function(value) {
      this.jenisPerhiasan = value;
    },
    onValueChangeKadar: function(value) {
      this.kadar = value;
    },
  },
  watch: {
    beratBersih: function () {
      this.debouncedApiCall()
    }
  },
};
</script>
