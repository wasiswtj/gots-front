<template>
  <view class="container">
    <app-loading v-if="!isAppReady"> </app-loading>
    <app v-if="isAppReady"></app>
  </view>
</template>

<script>
import Vue from "vue-native-core";
import { VueNativeBase } from "native-base";
import { AppLoading } from "expo";
import * as Font from "expo-font";
import axios from 'axios'
import querystring from 'querystring'
import Vuex from 'vuex'

import App from "../App.vue";

// registering all native-base components to the global scope of the Vue
Vue.use(VueNativeBase);
Vue.use(Vuex)

Vue.prototype.$http = axios;

Vue.config.productionTip = false

export const store = new Vuex.Store({
	state: {
  		status: '',
  		token: '',
  		user : {}
	},
	mutations: {
		auth_request(state){
	    	state.status = 'loading'
	  	},
	  	auth_success(state, token, user){
		    state.status = 'success'
		    state.token = token
		    state.user = user
	  	},
	  	auth_error(state){
	    	state.status = 'error'
	  	},
	  	logout(state){
	    	state.status = ''
	    	state.token = ''
	  	},
	},
	actions: {
	  	login({commit}, user){
			const requestBody = {
				email: user.email,
				password: user.password,
			}
			
	        return new Promise((resolve, reject) => {
	            commit('auth_request')
	            axios({url: 'http://192.168.43.13:9000/api/login', data: querystring.stringify(requestBody), method: 'POST', headers: {'Content-Type': 'application/x-www-form-urlencoded' }})
	            .then(resp => {
					const token = resp.data.token
	                axios.defaults.headers.common['Authorization'] = 'Bearer ' + token
					commit('auth_success', token)
					resolve(resp)
	            })
	            .catch(err => {
	                commit('auth_error', err)
	                reject(err)
	            })
	        })
	    },
	    register({commit}, user){
	    	return new Promise((resolve, reject) => {
	            commit('auth_request')
	            axios({url: 'http://localhost:3000/register', data: user, method: 'POST' })
	            .then(resp => {
	                const token = resp.data.token
	                const user = resp.data.user
	                localStorage.setItem('token', token)
	                // Add the following line:
	                axios.defaults.headers.common['Authorization'] = token
	                commit('auth_success', token, user)
	                resolve(resp)
	            })
	            .catch(err => {
	                commit('auth_error', err)
	                localStorage.removeItem('token')
	                reject(err)
	            })
	        })
	    },
	  	logout({commit}){
		    return new Promise((resolve, reject) => {
		      	commit('logout')
		      	localStorage.removeItem('token')
		      	delete axios.defaults.headers.common['Authorization']
		      	resolve()
		    })
	  	}
	},
	getters : {
	  isLoggedIn: state => !!state.token,
	  authStatus: state => state.status,
	}
})

export default {
  components: { App, AppLoading },
  data() {
    return {
      isAppReady: false
    };
  },
  created() {
    this.loadFonts();
  },
  methods: {
    async loadFonts() {
      try {
        this.isAppReady = false;
        await Font.loadAsync({
          Roboto: require("../../node_modules/native-base/Fonts/Roboto.ttf"),
          Roboto_medium: require("../../node_modules/native-base/Fonts/Roboto_medium.ttf"),
          Ionicons: require("../../node_modules/@expo/vector-icons/build/vendor/react-native-vector-icons/Fonts/Ionicons.ttf")
        });
        this.isAppReady = true;
      } catch (error) {
        console.log("some error occured", error);
        this.isAppReady = true;
      }
    }
  }
};
</script>

<style>
.container {
  flex: 1;
}
</style>
