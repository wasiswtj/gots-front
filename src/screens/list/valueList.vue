<template>
  <nb-content padder :style="{ marginTop: 0 }">
    
    <nb-card :style="{ marginBottom: 15 }">
      <nb-card-item header bordered>
        <nb-text>
          No Transaksi: {{dataTransaksi.no_transaksi}}
        </nb-text>
      </nb-card-item>

      <nb-card-item bordered>
        <nb-body>
          <nb-text>
            Jumlah Pinjaman: {{dataTransaksi.jumlah_pinjaman}}
          </nb-text>
          <nb-text>
            Status Transaksi: {{dataTransaksi.status_transaksi}}
          </nb-text>
          <nb-text>
            Tanggal Gadai: {{dataTransaksi.tanggal_gadai}}
          </nb-text>
        </nb-body>
      </nb-card-item>
    </nb-card>
  </nb-content>
</template>

<script>
export default {
  name: 'ValueList',
  props: ['dataTransaksi']
}    
</script>