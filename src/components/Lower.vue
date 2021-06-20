<template>
    <div class="container">
        <div class="streaming">
            <div class="wrap-data-player">
                <div class="banner">
                        <div class="logo-name">
                            <img :src="logoTeam" class="logo-team">
                            <p class="nickname">{{nickName}}</p>
                        </div>
                        <div>
                        <img :src="logoPlayer" class="player-avatar" alt="">
                        </div>
                        <div class="info-player">
                            <p>CAMPEÓN MÁS USADO</p>
                        </div>
                </div>
                <div class="grid">
                    <div class="col-data-player">
                        <div v-for="(data, index) in metrics" :key="index" class="col-progress-data">
                             <div class="col-name-metric">
                                <label for="file">{{labels[index]}}</label>
                             </div>
                             <div class="col-progress">
                                <div style="back">
                                 <progress id="file" :max="(data > 100) ? 500:50" :value="data"></progress>
                                </div>
                             </div>
                             <div class="col-number-metric">
                                 <label for="file">{{data}}</label>
                             </div>
                        </div>
                    </div>
                    
                    <div class="col-avatar-player">
                        <img src="https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Ornn_0.jpg" class="champion" alt="">
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
    grid-template-rows: 1px 300px;
}

.header-info-player {
    grid-row: 1 / 3;
    grid-column: 1 / 4;
    display: flex;
}

.banner{
    display: flex;
    justify-content: space-between;
    color: red;
    margin-left:auto;
}
.logo-name{
    text-align: center;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-left: 80px;
}
.nickname{
    float: right;
    color: white;
}

.info-player p{
    margin-top: 5%;
    font-size: 12px;
    color: white;
}

.header-info-player .name-player,
.header-info-player .info-player {
    color: white;
    width: 50%;
    padding: 8px 15px;
    font-weight: bold;
}

.info-player{
    margin-right: 2%;
}

.header-info-player .name-player {
    text-align: start;
} 

.header-info-player .info-player {
    text-align: end;
    font-size: 12px;
}

.col-data-player {
    grid-row: 2 / 3;
    grid-column: 1 / 2;
}
.col-progress-data{
    display: grid;
    margin-top: 5px;
}
.col-name-metric{
    grid-row: 1 / 1;
    grid-column: 1 / 1;
    width: 90px;
    text-align: right;
    font-size: 12px;
    color: #FFFFFF;
}
.col-progress{
    grid-row: 1 / 1;
    grid-column: 2 / 2;
    text-align: left;
    
}

progress::-webkit-progress-bar {background-color: #5801F0; width: 100%;}
progress {background-color: #5801F0;}

/* value: */
progress::-webkit-progress-value {background-color: #4AB800 !important;}
progress::-moz-progress-bar {background-color: #4AB800 !important;}
progress {
    color: #4AB800;
    height: 10px;
    margin-bottom: 5px;
}

.col-number-metric{
    grid-row: 3 / 1;
    grid-column: 3 / 3;
    text-align: left;
    font-size: 12px;
    width: 25px;
    color: #FFFFFF;
}

.logo-team{
    margin-right: 25%;
    height: auto;
    width: 28px;
}
 .col-image-player {
    grid-row: 1 / 3;
    grid-column: 2 / 3;
    z-index: 1;
    height: auto;
    max-width: 170px;
}
.col-image-player img{
    width: 125px;
}
    
.col-avatar-player {
    grid-row: 2 / 3;
    grid-column: 2 / 4;
    position: relative;
}

.col-avatar-player img{
    height: 105px;;
    max-width: 170px;
}
.player-avatar{
    position: absolute;
    z-index: 1;
    width: 125px;
    margin-left: 12%;
}

.champion{
    position:fixed;
}

p {
    margin: 0;
}

</style>