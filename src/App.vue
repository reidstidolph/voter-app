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
        <v-list-tile v-for="n in myUser.votes">
            <v-avatar size="45px">
              <img :src="myUser.gravatar">
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
              {{ props.item.votes.length }}
              <v-avatar size="45px" v-for="userId in props.item.votes">
                <img :src="users[userId].gravatar">
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
  import md5 from 'md5';
  import _ from 'lodash';

  export default {
    data () {
      return {
        myUser: {
          name: "Reid Stidolph",
          email: "reidstidolph@gmail.com",
          gravatar: null,
          votes: 5
        },
        users: {
          a1 : {
            name: "Bob Jones",
            email: "bob@foo.com",
            gravatar: null
          },
          b2 : {
            name: "Bill Bob",
            email: "bill@foo.com",
            gravatar: null
          },
          c3 : {
            name: "Jane Doe",
            email: "jane@foo.com",
            gravatar: null
          }
        },
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
              votes: [
                "a1"
              ]
            },
            {
              name: "Call of Duty: Modern Warfare 2",
              votes: [
                "a1",
                "b2"
              ]
            },
            {
              name: "Left4Dead",
              votes: [
                "a1",
                "b2",
                "c3"
              ]
            }
          ]
        }
      }
    },
    methods: {
      castVote(item){
        if (this.myUser.votes > 0) {
          item.votes +=1;
          this.myUser.votes -=1;
        }
      },
      retractVote(item){
        if (this.myUser.votes < 5 && item.votes > 0) {
          item.votes -=1;
          this.myUser.votes +=1;
        }
      },
      setAvatar(){
        console.log('getting user gravatar');
        this.myUser.gravatar=`https://www.gravatar.com/avatar/${md5(this.myUser.email)}?s=45`;
        _.forEach(this.users, (user)=>{
          user.gravatar=`https://www.gravatar.com/avatar/${md5(user.email)}?s=45`;
        });
      }
    },
    mounted() {
      this.setAvatar()
    }
  }
</script>
