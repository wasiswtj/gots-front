<template>
  <nb-content padder :style="{ marginTop: 0 }">
    <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item header bordered :style="{ borderRadius: 15 }">
        <nb-text>
          No Pengajuan: {{dataPengajuan.no_pengajuan}}
        </nb-text>
      </nb-card-item>
    </nb-card>

    <nb-card :style="{ borderRadius: 15, backgroundColor: '#FFF' }">
      <nb-button block light :onPress="revealCardImages" :style="{ elevation:0, flex:1, borderRadius: 15, backgroundColor: '#FFF' }" iconRight>
        <nb-text>Foto Agunan</nb-text>
        <nb-icon name="arrow-dropdown"/>
      </nb-button>

      <nb-card-item v-if="showCardImages==true" cardBody :style="{ borderRadius: 15 }">
        <image class="card-item-image"
          :style="{height: 300, flex:1, borderRadius: 15}"
          :source="{uri: 'data:image/jpeg;base64,' + dataPengajuan.foto_perhiasan}"
        />
      </nb-card-item>
    </nb-card>

    <!-- <nb-card :style="{ borderRadius: 15 }">
      
    </nb-card> -->

    <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item header bordered :style="{ borderRadius: 15 }">
        <nb-text>Informasi Penaksir</nb-text>
      </nb-card-item>
      <nb-card-item bordered :style="{ borderRadius: 15 }">
        <nb-left>
          <nb-thumbnail :source="logoPenaksir"></nb-thumbnail>
          <nb-body>
            <nb-text>{{dataPengajuan.nama_penaksir}} (Penaksir)</nb-text>
            <nb-text>No NIK: {{dataPengajuan.no_nik_penaksir}}</nb-text>
            <nb-text>No Handphone: {{dataPengajuan.no_handphone_penaksir}}</nb-text>
            <nb-text note>{{dataPengajuan.nama_outlet}}</nb-text>
            <nb-text note>{{dataPengajuan.no_telepon_cabang}}</nb-text>
          </nb-body>
        </nb-left>
      </nb-card-item>
      <nb-card-item bordered :style="{ borderRadius: 15 }">
        <nb-text :style="{fontWeight: 'bold'}">
          Penaksir anda akan tiba ditempat anda dalam {{dataPengajuan.estimasi_perjalanan}} menit.
        </nb-text>
      </nb-card-item>
    </nb-card>

    <!-- <nb-card :style="{ flex:1, borderRadius: 15}">
      <nb-card-item cardBody :style="{ borderRadius: 15 }">
        <image class="card-item-image"
          :style="{height: 300, flex:1, borderRadius: 15}"
          :source="{uri: 'data:image/jpeg;base64,' + dataPengajuan.foto_perhiasan}"
        />
      </nb-card-item>
    </nb-card> -->

    <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item header bordered :style="{ borderRadius: 15 }">
        <nb-text>Informasi Nasabah</nb-text>
      </nb-card-item>

      <nb-card-item bordered :style="{ borderRadius: 15 }">
        <nb-left>
          <nb-thumbnail :source="logoNasabah"></nb-thumbnail>
          <nb-body>
            <nb-text>
              No CIF: {{dataPengajuan.no_cif}}
            </nb-text>
            <nb-text>
              Nama: {{dataPengajuan.nama_nasabah}}
            </nb-text>
            <nb-text>
              Alamat: {{dataPengajuan.alamat}}
            </nb-text>
          </nb-body>
        </nb-left>
      </nb-card-item>
    </nb-card>

    <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item header bordered :style="{ borderRadius: 15 }">
        <nb-text>
          Informasi Barang
        </nb-text>
      </nb-card-item>

      <nb-card-item bordered :style="{ borderRadius: 15 }">
        <nb-body>
          <nb-text>
            Jenis Perhiasan: {{dataPengajuan.jenis_perhiasan}}
          </nb-text>
          <nb-text>
            Kadar: {{dataPengajuan.kadar}} karat
          </nb-text>
          <nb-text>
            Berat Kotor: {{dataPengajuan.berat_kotor}} gram
          </nb-text>
          <nb-text>
            Berat Bersih: {{dataPengajuan.berat_bersih}} gram
          </nb-text>
          <nb-text>
            Deskripsi Barang: {{dataPengajuan.keterangan_barang}}
          </nb-text>
        </nb-body>
      </nb-card-item>
    </nb-card>

    <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item class="container" :style="{ borderRadius: 15 }">
        <map-view class="map-style"
          :showsUserLocation="true"
          :region="{latitude: Number(dataPengajuan.cabang_latitude), longitude: Number(dataPengajuan.cabang_longitude), latitudeDelta: 0.043, longitudeDelta: 0.044}"
        >
          <marker
            :coordinate="{latitude: Number(dataPengajuan.cabang_latitude), longitude: Number(dataPengajuan.cabang_longitude)}"
            :title="dataPengajuan.nama_outlet"
            :description="dataPengajuan.alamat_cabang"
          />
        </map-view>
      </nb-card-item>
    </nb-card>
  </nb-content>
</template>

<script>
import { Dimensions, Platform, Alert, Text, View } from "react-native";
import React from 'react';
import MapView, { Marker } from 'react-native-maps';
import logoPenaksir from "../../../../assets/penaksir.png";
import logoNasabah from "../../../../assets/nasabah.png";

export default {
  components: { MapView, Marker },
  name: 'TabThree',
  props: ['dataPengajuan'],
  data() {
    return {
      logoPenaksir,
      logoNasabah,
      showCardImages: false
    }
  },
  methods: {
    revealCardImages() {
      if (this.showCardImages == true) {
        this.showCardImages = false
        return
      }
      this.showCardImages = true
    }
  }
}    
</script>

<style>
.container {
    flex: 1;
    background-color: #fff;
    align-items: center;
    justify-content: center;
}
.map-style {
  width: 300;
  height: 200;
}
</style>
