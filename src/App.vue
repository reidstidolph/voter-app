<template>
  <v-app>

<!--############################################################################
  Side nav drawer
#############################################################################-->
    <v-navigation-drawer
      fixed
      mini-variant
      clipped
      app
    >
      <v-flex>
        <v-avatar size="36px">
          <img src="https://www.gravatar.com/avatar/d8f74ac68d5a220a222372d2844729bc">
        </v-avatar>
      </v-flex>
    </v-navigation-drawer>

<!--############################################################################
  Top toolbar
#############################################################################-->
    <v-toolbar fixed app clipped-left>

    </v-toolbar>

<!--############################################################################
  Main content
#############################################################################-->
    <v-content>
      <v-container fluid>
        <v-layout column align-center>
          <img src="/public/v.png" alt="Vuetify.js" class="mb-5" />
          <blockquote>
            &#8220;First, solve the problem. Then, write the code.&#8221;
            <footer>
              <small>
                <em>&mdash;John Johnson</em>
              </small>
            </footer>
          </blockquote>
        </v-layout>
      </v-container>
    </v-content>

<!--############################################################################
  Bottom footer
#############################################################################-->
    <v-footer app>
      <span>&copy; 2017</span>
    </v-footer>


  </v-app>
</template>

<script>
  export default {
    data () {
      return {}
    }
  }
</script>
