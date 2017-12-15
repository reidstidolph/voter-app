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
      <v-list>
        <v-list-tile>
          Votes
        </v-list-tile>
        <v-list-tile v-for="n in myVotes">
            <v-avatar size="45px">
              <img src="https://www.gravatar.com/avatar/d8f74ac68d5a220a222372d2844729bc?s=45">
            </v-avatar>
        </v-list-tile>
      </v-list>


    </v-navigation-drawer>

<!--############################################################################
  Top toolbar
#############################################################################-->
    <v-toolbar fixed app clipped-left>
      <v-toolbar-title>Pollster</v-toolbar-title>
    </v-toolbar>

<!--############################################################################
  Main content
#############################################################################-->
    <v-content>
      <v-container fluid>
        <v-layout column align-center>
          <v-data-table
            v-bind:headers="gamesTable.headers"
            v-bind:pagination.sync="gamesTable.pagination"
            :items="gamesTable.items"
            class="elevation-1"
            hide-actions
            hide-headers
          >
          <template slot="items" slot-scope="props">
            <td style="width: 50px">
              <v-btn fab small @click.native="castVote(props.item)">
                <v-icon dark>add</v-icon>
              </v-btn>
              <v-btn fab small @click.native="retractVote(props.item)">
                <v-icon dark>remove</v-icon>
              </v-btn>
            </td>
            <td>
              {{ props.item.name }}
            </td>
            <td>
              {{ props.item.votes }}
              <v-avatar size="45px" v-for="n in props.item.votes">
                <img src="https://www.gravatar.com/avatar/d8f74ac68d5a220a222372d2844729bc?s=45">
              </v-avatar>
            </td>
          </template>
        </v-data-table>
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
      return {
        myVotes: 5,
        gamesTable: {
          pagination: {
            sortBy: 'votes',
            descending: true
          },
          headers: [
            {
              text: "Game Title",
              value: "name",
              sortable: false
            },
            {
              text: "Votes",
              value: "votes"
            }
          ],
          items: [
            {
              name: "Battlefield 4",
              votes: 1
            },
            {
              name: "Call of Duty: Modern Warfare 2",
              votes: 3
            },
            {
              name: "Left4Dead",
              votes: 2
            }
          ]
        }
      }
    },
    methods: {
      castVote(item){
        if (this.myVotes > 0) {
          item.votes +=1;
          this.myVotes -=1;
        }
      },
      retractVote(item){
        if (this.myVotes < 5 && item.votes > 0) {
          item.votes -=1;
          this.myVotes +=1;
        }
      }
    }
  }
</script>
