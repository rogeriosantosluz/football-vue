<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <table class="left">
        <h1>Championships</h1>
          <thead>
            <tr>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(championship) in championships" :key="championship.id"
              :class="{'highlight': (championship.id == selectedChampionship)}"
              v-on:click="getTeams(championship)">
              <td align="left">{{ championship.name}}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="col">
        <table class="left">
        <h1>Teams</h1>
          <thead>
            <tr>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(team) in teams" :key="team.id"
              :class="{'highlight': (team.id == selectedTeam)}"
              v-on:click="getPlayers(team)">
              <td align="left">{{ team.name}}</td>
            </tr>
          </tbody>
        </table>
      </div>
      <div class="col">
        <table class="left">
        <h1>Players</h1>
          <thead>
            <tr>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(player) in players" :key="player.id">
              <td align="left">
              {{ player.number}}-{{ player.name}} ({{ player.position}}) {{ player.nation}}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      championships: [],
      selectedChampionship: '',
      teams: [],
      selectedTeam: '',
      players: [],
    };
  },
  methods: {
    getPlayers(team) {
      this.selectedTeam = team.id;
      const path = 'http://localhost:5000/players';
      axios.get(path, {
        params: {
          team: team.id,
        },
      })
        .then((res) => {
          this.players = res.data.players;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
    getTeams(championship) {
      this.selectedChampionship = championship.id;
      const path = 'http://localhost:5000/teams';
      axios.get(path, {
        params: {
          championship: championship.id,
        },
      })
        .then((res) => {
          this.teams = res.data.teams;
          this.players = [];
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
    getChampionships() {
      const path = 'http://localhost:5000/championships';
      axios.get(path)
        .then((res) => {
          this.championships = res.data.championships;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
  },
  created() {
    this.getChampionships();
  },
};
</script>
<style>
.highlight {
     background-color: yellow;
}
tr:hover{
     cursor: pointer;
}
</style>
