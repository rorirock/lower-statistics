<template>
    <div class="container">
        <div class="streaming">
            <div class="wrap-data-player">
                <div class="grid">
                    <div class="header-info-player">
                        <div class="name-player">
                            <img :src="logoTeam">
                            <p>{{nickName}}</p>
                        </div>
                        <div class="info-player">
                            <p>CAMPEÓN MÁS USADO</p>
                        </div>
                    </div>
                    <div class="col-data-player">
                        <div v-for="(data, index) in metrics" :key="index">
                            <label for="file">{{labels[index]}}</label>
                            <progress id="file" max="500" value="363"></progress>
                            <label for="file">{{data}}</label>
                        </div>
                    </div>
                    <div class="col-image-player">
                        <img :src="logoPlayer" alt="">
                    </div>
                    <div class="col-avatar-player">
                        <img src="https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Ornn_0.jpg" alt="">
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    
  name: 'Lower',
  components: {
    
  },
  data(){
    return {
        labels:['KDA','CS/MIN','GOLD/MIN','DMG/MIN'],
        metrics: [],
        nickName:null,
        url:null,
        logoTeam:null,
        logoPlayer:null
    }
  },
  mounted () {
      this.url = process.env.VUE_APP_RUTE_API.replace(':competitionId',process.env.VUE_APP_COMPETITIONID).replace(':playerId',process.env.VUE_APP_PLAYERID);
      this.axios.get(this.url)
        .then((response)=>{
            console.log("data",response.data.data);
            let { kda,damagePerMinute,creepsPerMinute,goldPerMinute } = response.data.data.metrics;
            let { nickname, avatar } = response.data.data.player;
            let { original } = response.data.data.team.logo;
            this.metrics.push(kda,creepsPerMinute,goldPerMinute,damagePerMinute);
            this.nickName = nickname;
            this.logoTeam = original;
            this.logoPlayer= avatar.original;

            console.log(this.logoTeam);
            console.log(this.logoPlayer);
        })
        .catch(error => console.log(error))
  }
}
</script>

<style scoped>

body {
    font-family: 'Roboto', sans-serif;
}
.container {
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    overflow: hidden;
}

.container .streaming {
    height: 100%;
    background-color: green;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
}

.container .streaming .wrap-data-player {
    cursor: pointer;
    position: absolute;
    height: 135px;
    width: 700px;
    bottom: -105px;
    left: 50%;
    background: url('../assets/lower-background.png') center no-repeat;
    background-size: contain;
    transform: translateX(-50%);
    transition: .3s ease-in-out;
}

.container .streaming .wrap-data-player:hover {
    bottom: 0;
}

.container .streaming .wrap-data-player .grid {
    display: grid;
    grid-template-columns: 2fr 1fr 1fr;
    grid-template-rows: 30px 300px;
}

.header-info-player {
    grid-row: 1 / 2;
    grid-column: 1 / 4;
    display: flex;
}

.header-info-player .name-player,
.header-info-player .info-player {
    color: white;
    width: 50%;
    padding: 8px 15px;
    font-weight: bold;
}

.header-info-player .name-player {
    text-align: start;
}

.header-info-player .info-player {
    text-align: end;
}

.col-data-player {
    grid-row: 2 / 3;
    grid-column: 1 / 2;
}
.col-image-player {
    grid-row: 2 / 3;
    grid-column: 2 / 3;
    height: auto;
    max-width: 170px;
}
.col-avatar-player {
    grid-row: 2 / 3;
    grid-column: 3 / 4;
    position: relative;
}

.col-avatar-player img{
    height: auto;
    max-width: 170px;
}

p {
    margin: 0;
}

</style>