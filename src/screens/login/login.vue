<template>
  <nb-container :style="{ backgroundColor: '#fff' }">
    <nb-header>
      <!-- <nb-left>
        <nb-button transparent :onPress="() => this.props.navigation.goBack()">
          <nb-icon name="arrow-back" />
        </nb-button>
      </nb-left> -->
      <!-- <nb-left/> -->
      <!-- <nb-body :style="{ flex:1, marginLeft: 15, justifyContent: 'center' }"> -->
        <nb-title :style="{marginTop: 20}">GOTS Login</nb-title>
      <!-- </nb-body> -->
      <!-- <nb-right /> -->
    </nb-header>

    <nb-content padder>
      <nb-form>
        <nb-item floatingLabel>
          <nb-label>Email</nb-label>
          <nb-input v-model="email"/>
        </nb-item>
        <nb-item floatingLabel last>
          <nb-label>Password</nb-label>
          <nb-input secureTextEntry v-model="password"/>
        </nb-item>
      </nb-form>
      <nb-button :onPress="login" block :style="{ margin: 15, marginTop: 50 }">
        <nb-text>Sign In</nb-text>
      </nb-button>
    </nb-content>
  </nb-container>
</template>

<script>
import { Dimensions, Platform } from "react-native";
import { Toast } from "native-base";

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