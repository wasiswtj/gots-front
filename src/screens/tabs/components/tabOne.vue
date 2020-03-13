<template>
  <nb-content padder :style="{ marginTop: 0 }">
    <nb-card :style="{ borderRadius: 15 }">
      <nb-card-item header bordered :style="{ borderRadius: 15 }">
        <nb-text>
          No Pengajuan: {{dataPengajuan.no_pengajuan}}
        </nb-text>
      </nb-card-item>
    </nb-card>

    <nb-card :style="{ flex:1, borderRadius: 15}">
      <nb-card-item cardBody :style="{ borderRadius: 15 }">
        <image class="card-item-image"
          :style="{height: 300, flex:1, borderRadius: 15}"
          :source="{uri: 'data:image/jpeg;base64,' + dataPengajuan.foto_perhiasan}"
        />
      </nb-card-item>
    </nb-card>

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
    
    <nb-card :style="{ marginBottom: 15, borderRadius: 15 }">
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
import logoNasabah from "../../../../assets/nasabah.png";

export default {
  name: 'TabOne',
  props: ['dataPengajuan'],
  data() {
    return {
      logoNasabah,
    }
  }
}    
</script>