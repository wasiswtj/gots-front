<template>
  <nb-content padder :style="{ marginTop: 0 }">
    <nb-card :style="{ flex:1}">
      <nb-card-item cardBody :style="{ margin: 15 }">
        <image class="card-item-image"
          :style="{height: 300, flex:1}"
          :source="{uri: 'data:image/jpeg;base64,' + dataPengajuan.foto_perhiasan}"
        />
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
export default {
  name: 'TabOne',
  props: ['dataPengajuan']
}    
</script>