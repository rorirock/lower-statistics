<template>
    <div class="container">
        <div class="streaming">
                 <div class="content-player" id="player-content">
                    <img :src="logoPlayer" class="player-avatar" alt="">
                </div>
                <div class="wrap-data-player" id="wrap">
                    <div class="banner">
                            <div class="logo-name">
                                <img :src="logoTeam" class="logo-team">
                                <p class="nickname">{{nickName}}</p>
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
                                        <progress id="file" :max="laderMetrics[index]" :value="data"></progress>
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
        laderMetrics:[],
        nickName:null,
        url:null,
        urlLader:null,
        logoTeam:null,
        logoPlayer:null
    }
  },
  mounted () {
    this.urlLader = process.env.VUE_APP_LADDER.replace(':competitionId',process.env.VUE_APP_COMPETITIONID);
    this.url = process.env.VUE_APP_RUTE_API.replace(':competitionId',process.env.VUE_APP_COMPETITIONID).replace(':playerId',process.env.VUE_APP_PLAYERID);
    
    this.axios.get(this.urlLader)
        .then((resp)=>{
            let { kda,damagePerMinute,creepsPerMinute,goldPerMinute } = resp.data.data;
            this.laderMetrics.push(kda,creepsPerMinute,goldPerMinute,damagePerMinute);
    })
    .catch(error => console.log(error))

    this.axios.get(this.url)
        .then((response)=>{
            let { kda,damagePerMinute,creepsPerMinute,goldPerMinute } = response.data.data.metrics;
            let { nickname, avatar } = response.data.data.player;
            let { original } = response.data.data.team.logo;
            this.metrics.push(kda,creepsPerMinute,goldPerMinute,damagePerMinute);
            this.nickName = nickname.toUpperCase();
            this.logoTeam = original;
            this.logoPlayer= avatar.original;
    })
    .catch(error => console.log(error))

    setTimeout(function(){ 
       let wraphtml = document.getElementById('wrap');
       let player = document.getElementById('player-content');
        player.animate([
            {  bottom:'-30%',transform: 'translate(0,0)' },            
            {  bottom:'-60%',transform: 'translate(0,0)' }       
        ],{
            duration: 3000
        }
        )
        wraphtml.animate([
            {  bottom:'-4%',transform: 'translate(0,0)' },            
            { bottom:'-30%',transform: 'translate(0,0)' }       
        ],{
            duration: 3000
        }
        )
        player.style.bottom = '-60%'
        wraphtml.style.bottom = '-30%'
       
    }, 
    13000);
  },
}
</script>

<style scoped>


@keyframes slide {
        from{
            bottom:-30%;
            transform: translate(0,0);
        }             
        to{
            bottom:-4%;
            transform: translate(0,0);
        }        
}

@keyframes slide-player {
        from{
            bottom: -60%;
        }             
        to{
           height: 60%;
           width: 90%;
           bottom: -30%;
        }        
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
    height: 32.5%;
    width: 68.6%;
    left: 16%;
    bottom:-4%;
    background: url('../assets/lower-background.png') center no-repeat;
    background-size: contain;
    animation: slide 3s;
}

.container .streaming .wrap-data-player .grid {
    display: grid;
    grid-template-columns: 2fr 1fr;
    grid-template-rows: 0px 300px;
}
.content-player{
    cursor: pointer;
    position: absolute;
    height: 60%;
    width: 90%;
    bottom: -30%;
    z-index: 1;
    animation:slide-player 3s;
}

.header-info-player {
    grid-row: 1 / 3;
    grid-column: 1 / 4;
    display: flex;
}

.banner{
    display: flex;
    justify-content: space-between;
    margin-left:auto;
    margin-top: 3%;
}
.logo-name{
    text-align: center;
    display: flex;
    align-items: center;
    margin-left: 9%;
}
.nickname{
    float: right;
    font-size: 20px;
}


.header-info-player .name-player,
.header-info-player .info-player {
    color: white;
    width: 50%;
    padding: 8px 15px;
}

.info-player{
    display: flex;
    align-items: center;
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
    grid-template-columns: repeat(1, 1fr 3fr 1fr);
    margin-top:  2.7%;
}
.col-name-metric{
    grid-row: 1 / 1;
    grid-column: 1 / 1;
    width: 85%;
    text-align: right;
    font-size: 15px;
}
.col-progress{
    text-align: left;
}

progress::-webkit-progress-bar {background-color: #3f3f9f; width: 100%;}
progress {background-color: #3f3f9f;}

/* value: */
progress::-webkit-progress-value {background-color: #4AB800 !important;}
progress::-moz-progress-bar {background-color: #4AB800 !important;}
progress {
    color: #4AB800;
    width: 95%;
    height: 10px;
    margin-bottom: 5px;
}

.col-number-metric{
    text-align: left;
    font-size: 20px;
}

.logo-team{
    margin-right: 2%;
    height: auto;
    width: 6%;
}
 .col-image-player {
    grid-row: 1 / 3;
    grid-column: 2 / 3;
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

.player-avatar{
    position: absolute;
    z-index: 1;
    width: 300px;
    margin-left: 12%;
}

.champion{
    width: 90%;
    margin-left: 10%;
    margin-top: 1%;
}

p {
    margin: 0;
}


</style>