<template>
  <nb-content padder :style="{ marginTop: 0 }">
     <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item header bordered :style="{ borderRadius: 15 }">
        <nb-text>
          No Pengajuan: {{dataPengajuan.no_pengajuan}}
        </nb-text>
      </nb-card-item>
    </nb-card>

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
        <nb-text note>Penaksir anda telah tiba ditempat anda. Selamat menikmati kenyamanan bertransaksi menggunakan layanan gade on the spot.</nb-text>
      </nb-card-item>
    </nb-card>

    <nb-card :style="{ marginBottom: 15, borderRadius: 15  }">
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
  </nb-content>
</template>

<script>
import logoPenaksir from "../../../../assets/penaksir.png";

export default {
  name: 'TabFour',
  props: ['dataPengajuan'],
  data() {
    return {
      logoPenaksir,
    }
  }
}    
</script>