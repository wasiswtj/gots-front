<template>
  <nb-container :style="{ backgroundColor: 'white' }">
    <!-- <nb-thumbnail square large :source="logoGots" :style="{ margin: 15}"></nb-thumbnail> -->
    
    <!-- <nb-title  :style="{ color: '#3CB371',marginTop:50 }">GOTS Login</nb-title> -->
    <nb-content padder>
      <image
        :style="{width:220, height:160,flex:1, marginTop:50, marginLeft:65, marginBottom:30
        }"
        :source="logoGots"
      />
      <nb-form >
        <nb-item floatingLabel :style="{ margin: 15}">
          <nb-label>Nomor CIF</nb-label>
          <nb-input v-model="email"/>
        </nb-item>
        <nb-item floatingLabel :style="{ margin: 15}">
          <nb-label>Password</nb-label>
          <nb-input secureTextEntry v-model="password"/>
        </nb-item>
      </nb-form>
      <nb-button rounded success :onPress="login" block :style="{ margin: 15, marginTop: 30 }">
        <nb-text>Log In</nb-text>
      </nb-button>
      <nb-footer transparent :style="{ backgroundColor: 'white',marginTop:110, elevation:0 }">
        <text>© PT Aplikasi Karya Anak Six Fams</text>
      </nb-footer>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform } from "react-native";
import { Toast } from "native-base";
import logoGots from "../../../assets/logogots.png";

export default {
  props: {
    navigation: {
      type: Object
    }
  },
  computed: {
    logging_in () {
      return store.state.logging_in;
    }
  },
  data() {
    return {
      email: '',
      password: '',
      loaded: false,
      logoGots
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
  methods: {
    login: function () {
      let email = this.email 
      let password = this.password

      this.$store.dispatch('login', { email, password })
        .then(() => {
          this.navigation.navigate("Home")
        })
        .catch(err => {
          console.log(err)
          this.showWrongCredentialsWarning()
        })
    },
    showWrongCredentialsWarning() {
      Toast.show({
        text: "Nomor CIF atau Password anda salah!",
        buttonText: "Okay",
        type: "danger"
      });
    }
    // login() {
    //   if (this.emailValue && this.password && !this.$v.emailValue.$invalid) {
    //     store.dispatch('LOGIN', {
    //       userObj: {email: this.emailValue},
    //       navigate: this.navigation.navigate
    //     });
    //   } else {
    //     Toast.show({
    //       text: 'Invalid Email or Password',
    //       buttonText: 'Okay'
    //     })
    //   }
    // }
  }
};
</script>