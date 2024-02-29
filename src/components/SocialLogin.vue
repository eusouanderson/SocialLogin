<template>
    <div class="social-login">
      <button v-if="!isLoggedIn" @click="SignUpUsingGoogle" type="button" class="login-btn google">
        <i class="fab fa-google" style="margin-right: 5px;"></i> Sign in with Google
      </button>
      <button v-if="!isLoggedIn" @click="SignUpUsingFacebook" type="button" class="login-btn facebook">
        <i class="fab fa-facebook" style="margin-right: 5px;"></i> Sign in with Facebook
      </button>
  
      <div v-if="isLoggedIn" class="wrapper">
        <div class="profile-card js-profile-card">
          <div class="profile-card__img">
            <img :src="userData.photoURL" alt="profile card" />
          </div>
          <div class="profile-card__cnt js-profile-cnt">
            <div class="profile-card__name">{{ userData.displayName }}</div>
            <div class="profile-card__txt">{{ userData.email }}</div>
            <div class="profile-card__txt">ID: {{ userData.uid }}</div>
            <div class="profile-card-loc"></div>
            <div class="profile-card-ctr">
              <button class="profile-card__button button--orange" @click="ForgetLogin">Forget Login</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import { onAuthStateChanged, signInWithPopup, GoogleAuthProvider, FacebookAuthProvider } from "firebase/auth";
  import { auth } from "../firebase/firebaseConfig";
  
  export default {
    data() {
      return {
        isLoggedIn: false,
        userData: {}
      };
    },
    mounted() {
      const unsubscribe = onAuthStateChanged(auth, (result) => {
        if (result) {
          const { displayName, email, uid } = result;
          this.userData = { displayName, email, uid };
          this.isLoggedIn = true;
        } else {
          this.isLoggedIn = false;
        }
      });
  
      this.unsubscribeOnUnmount = unsubscribe;
    },
    beforeUnmount() {
      if (this.unsubscribeOnUnmount) {
        this.unsubscribeOnUnmount();
      }
    },
    methods: {
      SignUpUsingGoogle() {
        const provider = new GoogleAuthProvider();
  
        signInWithPopup(auth, provider)
          .then((result) => {
            const { displayName, email, uid } = result.user;
            this.userData = { displayName, email, uid };
            this.isLoggedIn = true;
          })
          .catch((error) => {
            console.log({ error });
          });
      },
      SignUpUsingFacebook() {
        const provider = new FacebookAuthProvider();
  
        signInWithPopup(auth, provider)
          .then((result) => {
            const { displayName, email, uid } = result.user;
            this.userData = { displayName, email, uid };
            this.isLoggedIn = true;
          })
          .catch((error) => {
            console.log({ error });
          });
      },
      ForgetLogin() {
        this.userData = {};
        this.isLoggedIn = false;
      }
    }
  };
  </script>
  
  <style scoped>
  
  </style>
  