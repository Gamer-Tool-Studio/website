<script lang="ts" setup>
import { storeToRefs } from 'pinia'

// import storeToRefs helper hook from pinia
import { useAuthStore } from '~/store/auth'

// import the auth store we just created
import { getWordPattern } from '~/utils'

const { getUserProfile } = useUserStore()

const debug = getDebugger('AppHeader')

const route = useRoute()

const isLandingPage = computed(() => ['/', '/cluaido', '/cluaido/'].includes(route.path))

const pattern = /^(\/[^\/]+)/
const smallHeaderPages = ['/dashboard']

const isSmallHeader = computed(() => !smallHeaderPages.includes(getWordPattern(route?.path || '', pattern)))

const openDemoGames = ref(false)
const openProducts = ref(false)

// authentication methods

const { authenticated } = storeToRefs(useAuthStore()) // make authenticated state reactive with storeToRefs

if (authenticated.value) {
  debug.log('authenticated', authenticated.value)

  getUserProfile()
}
</script>

<template>
  <nav :class="{ lp: isLandingPage, small: isSmallHeader }">
    <div class="navbar">
      <ul class="nav-links">
        <li class="nav-left">
          <NuxtLink to="/">
            <img alt="logo" src="~/assets/images/GTS-iso.png" class="nav-logo">
          </NuxtLink>
        </li>
        <li class="nav-left mobile-display">
          <!-- <NuxtLink to="/products">Products</NuxtLink> -->
          <NuxtLink>
            Products
            <v-menu v-model:model-value="openProducts" activator="parent">
              <v-list class="dropDownMenu">
                <a class="dropLink" href="/#sectionToLinkToApi"><v-list-item class="dropDownItem"> SDK</v-list-item></a>
                <a class="dropLink" href="/#sectionToLinkToPlugin"><v-list-item class="dropDownItem"> Plugins</v-list-item></a>
                <a class="dropLink" href="https://web3bazaar.org"><v-list-item class="dropDownItem"> Web3 Marketplace</v-list-item></a>
              </v-list>
            </v-menu>
          </NuxtLink>
        </li>
        <li class="nav-left mobile-display">
          <NuxtLink to="https://gamertoolstudio.gitbook.io/npc-gpt/introduction/introduction">
            Documentation
          </NuxtLink>
        </li>
        <li class="nav-left mobile-display">
          <NuxtLink to="/pricing">
            Pricing
          </NuxtLink>
        </li>
        <li class="nav-left mobile-display">
          <NuxtLink to="/download-plugin">
            Download Plugins
          </NuxtLink>
        </li>
        <li class="nav-left mobile-display">
          <NuxtLink>
            Demo Games
            <v-menu v-model:model-value="openDemoGames" activator="parent">
              <v-list class="dropDownMenu">
                <a class="dropLink" href="/cluaido"><v-list-item class="dropDownItem"> CLUAIDO</v-list-item></a>
              </v-list>
            </v-menu>
          </NuxtLink>
        </li>
        <!--       <li class="nav-left mobile-display">
          <NuxtLink to="/mint">
            Mint
          </NuxtLink>
        </li>
      -->
      </ul>
      <ul class="login-section">
        <li v-if="!authenticated" class="nav-right-link d-none d-sm-flex">
          <NuxtLink to="/login">
            Log in
          </NuxtLink>
        </li>
        <li v-if="!authenticated" class="nav-right d-none d-sm-flex">
          <NuxtLink to="/login?register=true" class="button">
            Sign up
          </NuxtLink>
        </li>
        <li class="user-menu" :class="{ 'd-sm-none': !authenticated }">
          <DashboardUserMenu :is-landing-page="isLandingPage" />
          <!-- <NuxtLink @click="logout">Logout</NuxtLink> -->
        </li>
      </ul>
    </div>
  </nav>
</template>

<style lang="scss">
nav {
  width: 100%;
  height: 100%;
  margin: 0 auto;
  position: fixed;
  overflow: hidden;
  display: flex;
  align-content: center;
  justify-content: center;
  background-color: transparent;
  color: #333;
  padding: 10px 0;

  /* Navigation Bar Styles */
  .navbar {
    max-height: 55px;
    width: 100%;
    display: flex;
    justify-content: center;

    .login-section li {
      margin: 0 12px;
    }
    .nav-links {
      padding-left: 16px;
      list-style: none;
      display: inline-flex;
      align-items: center;
      a {
        cursor: pointer;
        color: #000;
        text-decoration: none;
        font-size: 22px;
        font-weight: 100;
        transition: color 0.3s ease;
        :hover {
          color: #6200ee;
        }
      }
    }
    a {
      color: black;
      text-decoration: none;
      font-family: 'Bebas Neue';
      &.button {
        color: #fff;
      }
    }
    .nav-logo {
      margin-right: 10px;
      padding: 0;
      display: flex;
      flex-wrap: wrap;
      height: 100%;
    }

    img.nav-logo {
      display: block;
      max-height: 40px;
      max-width: 40px;
      // margin-bottom: 15px;
    }
  }

  .nav-links .nav-link:hover {
    text-decoration: underline;
    cursor: pointer;
  }

  .login-section .nav-link:hover {
    text-decoration: underline;
  }

  /* New Code to Align Login and Signup to Right */
  .login-section {
    margin-left: auto;
    font-size: 22px;

    // .button {
    //   @at-root #{selector.unify(&, a)} {
    //     color: white;
    //     // padding-bottom: 1px;
    //   }
    // }
  }

  .login-section {
    margin: 0;
    margin-left: auto;
    padding: 0;
    list-style: none;
    display: flex;
    align-items: center;
    font-family: 'Bebas Neue';
  }

  .nav-right a {
    font-size: 22px;
  }
  .nav-right:hover {
    cursor: pointer;
  }

  .nav-right a:hover {
    color: #6200ee !important;
    background-color: #fff;
  }

  .nav-right-link {
    margin-left: 0 !important;

    display: flex;
    align-items: center;
    font-family: 'Bebas Neue';
  }

  .nav-right-link img {
    width: 30px;
    height: 30px;
    border-radius: 50%;
  }

  .nav-right-link:hover {
    // text-decoration: underline;
    cursor: pointer;
    text-decoration: underline;
  }
}

.mobile-menu {
  display: none;
}

nav.small {
  .navbar {
    max-width: 1200px;
  }
}

nav.lp {
  background-color: black;

  * {
    color: white;
  }
  .nav-links a,
  a {
    color: #fff;
    text-decoration: none;
  }
  .button {
    display: inline-block;
    background-color: black;
    color: #fff;
    padding: 5px 15px 1px 15px;
    text-decoration: none;
    border: 1px solid #fff;
    border-radius: 5px;
    font-size: 22px;
    transition: background-color 0.3s ease;
    &:hover {
      background-color: #fff;
      color: black;
    }
  }
}

/* Media Queries for Responsiveness */
@media (max-width: 820px) {
  .mobile-display {
    display: none !important;
  }
  // .login-section li {
  //   display: flex !important;
  //   align-items: center !important;
  // }

  .mobile-menu {
    display: flex;
    align-items: center;
    font-size: 50px !important;
  }

  .mobile-menu:hover {
    cursor: pointer;
  }

  // .nav-right-link:not(.user-menu) {
  //   display: none !important;
  // }
}

@media (max-width: 930px) {
  .nav-links a {
    font-size: 16px !important;
  }
}

@media (max-width: 600px) {
  .user-menu button {
    padding-bottom: 55px;
    font-size: 35px !important;
  }
}
</style>
src/utils
