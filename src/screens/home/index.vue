<template>
  <nb-container :style="{ backgroundColor: '#e3e3e3' }">
    <nb-header>
      <nb-left> </nb-left>
      <nb-body>
        <nb-title> Gade On The Spot</nb-title>
      </nb-body>
    </nb-header>

    <nb-content padder class="content-wrapper">
      <nb-card :style="{ marginBottom: 200 }">
        <nb-card-item>
          <nb-left>
            <nb-thumbnail :source="cover"></nb-thumbnail>
            <nb-body>
              <nb-text>Hello</nb-text>
              <nb-text note>GeekyAnts</nb-text>
            </nb-body>
          </nb-left>
        </nb-card-item>
      </nb-card>

      <nb-button rounded success :onPress="() => this.props.navigation.navigate(PengajuanPerhiasan)" :style="{ flex: 1, marginBottom: 10 }">
        <nb-text>Ajukan Gadai Sekarang</nb-text>
      </nb-button>

      <nb-card :style="{ marginBottom: 15 }">
        <nb-card-item header bordered>
          <nb-text :style="{ color: 'green' }">Menu Anda</nb-text>
        </nb-card-item>

        <nb-card-item>
          <nb-left/>
          <view class="view-wrapper-1" :style="{ marginTop:20 }">
            <nb-list :style="{justifyContent: 'center', alignItems: 'center', margin:10 }">
              <nb-button transparent :onPress="() => this.props.navigation.navigate(StatusPengajuan)" padded>
                <nb-thumbnail large :source="logoPengajuan"></nb-thumbnail>
              </nb-button>
              <nb-text/>
              <nb-text>Pengajuan</nb-text>
              <nb-text>Anda</nb-text>
            </nb-list>
            <nb-list :style="{justifyContent: 'center', alignItems: 'center', margin:10 }">
              <nb-button transparent :onPress="checkState" padded>
                <nb-thumbnail large :source="cover"></nb-thumbnail>
              </nb-button>
              <nb-text/>
              <nb-text>Transaksi</nb-text>
              <nb-text>Anda</nb-text>
            </nb-list>
            <nb-list :style="{justifyContent: 'center', alignItems: 'center', margin:10 }">
              <nb-button transparent :onPress="() => this.props.navigation.navigate()" padded>
                <nb-thumbnail large :source="cover"></nb-thumbnail>
              </nb-button>
              <nb-text/>
              <nb-text>Riwayat</nb-text>
              <nb-text>Anda</nb-text>
            </nb-list>
          </view>
          <nb-right/>
        </nb-card-item>
      </nb-card>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform } from "react-native";
import launchScreenBg from "../../../assets/launchscreen-bg.png";
import launchscreenLogo from "../../../assets/logo-kitchen-sink.png";
import cover from "../../../assets/web-cover1.jpg";
import logoPengajuan from "../../../assets/logo-pengajuan.png";

export default {
  props: {
    navigation: {
      type: Object
    }
  },
  created() {
    this.navigation.navigate("Login")
    
    // if (store.state.token!=null) {
    //   this.navigation.navigate("StatusPengajuan")
    // }
    // } catch (err) {} finally {
    //   this.navigation.navigate(this.data.StatusPengajuan)
    // }
    // if (typeof(this.data.token)=='undefined') {
    //   console.log('throughThis')
    //   this.navigation.navigate(this.data.StatusPengajuan)
    // }
  },
  data() {
    return {
      PengajuanPerhiasan: "PengajuanPerhiasan",
      StatusPengajuan: "StatusPengajuan",
      launchScreenBg,
      launchscreenLogo,
      stylesObj: {
        logoContainerStyle: {
          marginTop: Dimensions.get("window").height / 8
        },
        logoStyle: {
          left: Platform.OS === "android" ? 40 : 50,
          top: Platform.OS === "android" ? 35 : 60
        },
        btnContainer: {
          backgroundColor: "#6faf98",
          alignSelf: "center"
        }
      },
      cover,
      logoPengajuan,
      token: this.$store.state.token
    };
  },
  methods: {
    handleLetGoBtnPress() {
      this.navigation.openDrawer();
    },
    login: function () {
      let email = this.email 
      let password = this.password
      console.log(email)
      store.dispatch('login', { email, password })
      .catch(err => console.log(err))
    },
    checkState: function () {
      console.log(this.$store.state.token)
    }
  }
};
</script>

<style>
.imageContainer {
  flex: 1;
}
.text-color-primary {
  color: blue;
  font-family: Roboto;
}
.logoContainer {
  flex: 1;
  margin-bottom: 30;
}
.logo {
  position: absolute;
  width: 280;
  height: 100;
}
.text-container {
  align-items: center;
  margin-bottom: 50;
  background-color: transparent;
}
.text-color-white {
  color: white;
}
.button-container {
  background-color: #6faf98;
  align-self: center;
}
.content-wrapper {
  background-color: #fff;
}
.view-wrapper-1 {
  flex-direction: row;
  justify-content: space-between;
}
.view-wrapper-2 {
  flex-direction: row;
}
</style>