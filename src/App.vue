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
              <img :src="users[myUser.id].gravatar">
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
              <v-btn v-if="!props.item.myVote && myUser.votes > 0" fab small @click.native="castVote(props.item)">
                <v-icon dark>add</v-icon>
              </v-btn>
              <v-btn v-if="props.item.myVote" fab small @click.native="retractVote(props.item)">
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

  const userVoteCount = 3;

  var vueState = {
    myUser: {
      id : "d4",
      votes: userVoteCount
    },
    users: {
      a1 : {
        name: "Lambert Green",
        email: "lambert_green@msn.com",
        gravatar: null
      },
      b2 : {
        name: "Ryan Hellevang",
        email: "ryan@hellevang.us",
        gravatar: null
      },
      c3 : {
        name: "Jane Doe",
        email: "jane@foo.com",
        gravatar: null
      },
      d4 : {
        name: "Reid Stidolph",
        email: "reidstidolph@gmail.com",
        gravatar: null,
      }
    },
    gamesTable: {
      pagination: {
        sortBy: 'votes.length',
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
          myVote: false,
          votes: [
            "a1"
          ]
        },
        {
          name: "Call of Duty: Modern Warfare 2",
          myVote: false,
          votes: [
            "a1",
            "b2"
          ]
        },
        {
          name: "Left4Dead",
          myVote: false,
          votes: [
            "a1",
            "b2",
            "c3"
          ]
        },
        {
          name: "Age of Empires",
          myVote: false,
          votes: [
            "a1",
            "b2",
            "c3"
          ]
        },
        {
          name: "Overwatch",
          myVote: false,
          votes: [
            "c3"
          ]
        },
        {
          name: "PlayerUnknown's Battlegrounds",
          myVote: false,
          votes: [
            "a1",
            "c3"
          ]
        }
      ]
    }
  }

  export default {
    data () {
      return vueState;
    },
    methods: {
      castVote(item){
        if (this.myUser.votes > 0 && !_.includes(item.votes, this.myUser.id)) {
          item.votes.push(this.myUser.id);
          item.myVote = true;
          this.myUser.votes -=1;
        }
      },
      retractVote(item){
        if (this.myUser.votes < userVoteCount && _.includes(item.votes, this.myUser.id)) {
          _.pull(item.votes, this.myUser.id);
          item.myVote = false;
          this.myUser.votes +=1;
        }
      },
      setAvatar(){
        console.log('getting user gravatar');
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
