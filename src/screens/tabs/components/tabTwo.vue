<template>
  <nb-content padder :style="{ marginTop: 0 }">
    <nb-card>
        <nb-card-item>
          <nb-left>
            <nb-thumbnail :source="logoPenaksir"></nb-thumbnail>
            <nb-body>
              <nb-text>{{dataPengajuan.nama_penaksir}}</nb-text>
              <nb-text note>Cabang Kemanggisan</nb-text>
            </nb-body>
          </nb-left>
        </nb-card-item>
    </nb-card>

    <nb-card :style="{ marginBottom: 15 }">
        <nb-card-item header bordered>
          <nb-text>
            No Pengajuan: {{dataPengajuan.no_pengajuan}}
          </nb-text>
        </nb-card-item>

        <nb-card-item bordered>
          <nb-body>
            <nb-text>
              Jenis Perhiasan: {{dataPengajuan.jenis_perhiasan}}
            </nb-text>
            <nb-text>
              Kadar: {{dataPengajuan.kadar}}
            </nb-text>
            <nb-text>
              Berat Kotor: {{dataPengajuan.berat_kotor}}
            </nb-text>
            <nb-text>
              Berat Bersih: {{dataPengajuan.berat_bersih}}
            </nb-text>
          </nb-body>
        </nb-card-item>
      </nb-card>
  </nb-content>
</template>

<script>
import logoPenaksir from "../../../../assets/penaksir.png";

export default {
  name: 'TabTwo',
  props: ['dataPengajuan'],
  data() {
    return {
      logoPenaksir,
    }
  }
}    
</script>