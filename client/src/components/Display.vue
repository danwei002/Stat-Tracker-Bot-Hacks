<template>
<section>
  <div v-if="loading">
    <body>
      <div class="preloader">
        <div class="SpanLoad">
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
          <span></span>
        </div>
      </div>
    </body>
  </div>

  <div v-if="error">
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-4"></div>

        <div class="col-md-4 error-box">
          <h1 class="error" style="text-align: center;">
            ERROR 404
            <br />
            {{ error }}
          </h1>
          <br />
          <router-link class="erbt" style="align" to="/">Go Back</router-link>
        </div>
      </div>
    </div>
  </div>

  <div v-if="profileData">
    <div class="container-fluid">
      <div class="row">
        <h1 class="gamertag">
          {{ profileData.platformInfo.platformUserId}}
          <i
            class="fab fa-battle-net ml-3"
            v-if="this.$route.params.platform == 'battlenet'"
            style="color: #00a6ff;"
          ></i>
          <i
            class="fab fa-xbox ml-3"
            v-if="this.$route.params.platform == 'xbl'"
            style="color: #14e326;"
          ></i>
          <i
            class="fab fa-playstation ml-3"
            v-if="this.$route.params.platform == 'psn'"
            style="color: #00a6ff;"
          ></i>
        </h1>
      </div>
    </div>
    <div class="container-fluid mb-3">
      <div class="row stats-top-title ml-2 mr-2">
        <h1 class="m-3" style="text-align: center; width: 100%;">CAREER STATS</h1>
      </div>
      <div class="row stats-top-content ml-2 mr-2">
        <div class="col-md-2 mt-3">
          <h4 class="stats-title">Time Played</h4>
          <h5
            v-if="
                profileData.segments[0].stats.timePlayed.value &&
                  profileData.segments[1].stats.timePlayed.value
              "
            class="stats-desc"
          >{{convertTime(profileData.segments[0].stats.timePlayed.value + profileData.segments[1].stats.timePlayed.value)}}</h5>
          <h5
            class="stats-desc"
            v-else-if="
                profileData.segments[0].stats.timePlayed.value &&
                  !profileData.segments[1].stats.timePlayed.value
              "
          >{{convertTime(profileData.segments[0].stats.timePlayed.value)}}</h5>

          <h5
            class="stats-desc"
            v-else
          >{{convertTime(profileData.segments[1].stats.timePlayed.value)}}</h5>
        </div>
        <div class="col-md-2 mt-3">
          <h4 class="stats-title">Total Time Spent On Fire</h4>
          <h5
            v-if="
                profileData.segments[0].stats.timeSpentOnFire.value &&
                  profileData.segments[1].stats.timeSpentOnFire.value
              "
            class="stats-desc"
          >{{ convertTime(profileData.segments[0].stats.timeSpentOnFire.value+profileData.segments[1].stats.timeSpentOnFire.value) }}</h5>
          <h5
            class="stats-desc"
            v-else-if="
                !profileData.segments[0].stats.timeSpentOnFire.value &&
                  profileData.segments[1].stats.timeSpentOnFire.value
              "
          >{{ convertTime(profileData.segments[1].stats.timeSpentOnFire.value) }}</h5>
          <h5
            class="stats-desc"
            v-else-if="
                profileData.segments[0].stats.timeSpentOnFire.value &&
                  !profileData.segments[1].stats.timeSpentOnFire.value
              "
          >{{ convertTime(profileData.segments[0].stats.timeSpentOnFire.value) }}</h5>
          <h5 v-else class="stats-desc">N/A</h5>
        </div>
        <div class="col-md-2 mt-3">
          <h4 class="stats-title">Total Time On Objective</h4>
          <h5
            v-if="
                profileData.segments[0].stats.objectiveTime.value &&
                  profileData.segments[1].stats.objectiveTime.value "
            class="stats-desc"
          >{{ convertTime(profileData.segments[0].stats.objectiveTime.value+profileData.segments[1].stats.objectiveTime.value) }}</h5>
          <h5
            v-else-if="
                profileData.segments[0].stats.objectiveTime.value &&
                  !profileData.segments[1].stats.objectiveTime.value
              "
            class="stats-desc"
          >{{ convertTime(profileData.segments[0].stats.objectiveTime.value) }}</h5>
          <h5
            v-else-if="
                !profileData.segments[0].stats.objectiveTime.value &&
                  profileData.segments[1].stats.objectiveTime.value
              "
            class="stats-desc"
          >{{ convertTime(profileData.segments[1].stats.objectiveTime.value) }}</h5>
          <h5 v-else class="stats-desc">N/A</h5>
        </div>
        <div class="col-md-2 mt-3">
          <h4 class="stats-title">Win Rate</h4>
          <h5
            v-if="
                profileData.segments[0].stats.wlPercentage.value &&
                  profileData.segments[1].stats.wlPercentage.value
              "
            class="stats-desc"
          >
            {{
            (profileData.segments[0].stats.wlPercentage.value +
            profileData.segments[1].stats.wlPercentage.value) /
            2
            }}%
          </h5>
          <h5
            v-else-if="
                profileData.segments[0].stats.wlPercentage.value &&
                  !profileData.segments[1].stats.wlPercentage.value
              "
            class="stats-desc"
          >{{ profileData.segments[0].stats.wlPercentage.value }}%</h5>
          <h5
            v-else-if="
                !profileData.segments[0].stats.wlPercentage.value &&
                  profileData.segments[1].stats.wlPercentage.value
              "
            class="stats-desc"
          >{{ profileData.segments[1].stats.wlPercentage.value }}%</h5>
          <h5 v-else class="stats-desc">N/A</h5>
        </div>
        <div class="col-md-2 mt-3">
          <h4 class="stats-title">Games Won</h4>
          <h5
            v-if="
                profileData.segments[0].stats.wins.value &&
                  profileData.segments[1].stats.wins.value
              "
            class="stats-desc"
          >
            {{
            profileData.segments[0].stats.wins.value +
            profileData.segments[1].stats.wins.value
            }}
          </h5>
          <h5
            v-else-if="
                profileData.segments[0].stats.wins.value &&
                  !profileData.segments[1].stats.wins.value
              "
            class="stats-desc"
          >{{ profileData.segments[0].stats.wins.value }}</h5>
          <h5
            v-else-if="
                !profileData.segments[0].stats.wins.value &&
                  profileData.segments[1].stats.wins.value
              "
            class="stats-desc"
          >{{ profileData.segments[1].stats.wins.value }}</h5>
          <h5 v-else class="stats-desc">N/A</h5>
        </div>
        <div class="col-md-2 mt-3">
          <h4 class="stats-title">Most Eliminations</h4>
          <h5
            v-if="
                profileData.segments[0].stats.mostEliminations.value &&
                  profileData.segments[1].stats.mostEliminations.value &&
                  profileData.segments[0].stats.mostEliminations.value >
                    profileData.segments[1].stats.mostEliminations.value
              "
            class="stats-desc"
          >{{ profileData.segments[0].stats.mostEliminations.value }}</h5>
          <h5
            v-else-if="
                profileData.segments[0].stats.mostEliminations.value &&
                  profileData.segments[1].stats.mostEliminations.value &&
                  profileData.segments[0].stats.mostEliminations.value <
                    profileData.segments[1].stats.mostEliminations.value
              "
            class="stats-desc"
          >{{ profileData.segments[1].stats.mostEliminations.value }}</h5>
          <h5
            v-else-if="
                profileData.segments[0].stats.mostEliminations.value &&
                  profileData.segments[1].stats.mostEliminations.value &&
                  profileData.segments[0].stats.mostEliminations.value ===
                    profileData.segments[1].stats.mostEliminations.value
              "
            class="stats-desc"
          >{{ profileData.segments[1].stats.mostEliminations.value }}</h5>
          <h5
            v-else-if="
                profileData.segments[0].stats.mostEliminations.value &&
                  !profileData.segments[1].stats.mostEliminations.value
              "
            class="stats-desc"
          >{{ profileData.segments[0].stats.mostEliminations.value }}</h5>
          <h5
            v-else-if="
                !profileData.segments[0].stats.mostEliminations.value &&
                  profileData.segments[1].stats.mostEliminations.value
              "
            class="stats-desc"
          >{{ profileData.segments[1].stats.mostEliminations.value }}</h5>
        </div>
      </div>
    </div>
    <div class="container-fluid">
      <div class="row">
        <div class="col-md-6 col-sm-12 col-xs-12">
          <div class="stats-casual-content ml-2 mr-2">
            <h5 class="w-100 pt-2" id="casual-title" style="text-align: center;">QUICKPLAY</h5>
            <hr style="background-color: #9dacbf; height: 3px;" />
            <h5 class="w-100 text-center" style="font-size: 1.75rem;">
              <u>Overall</u>
            </h5>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Win Rate:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[0].stats.wlPercentage.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Eliminations:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[0].stats.eliminations.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Deaths:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[0].stats.deaths.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  K/D Ratio:
                  <span
                    class="stat-d"
                  >{{ Math.round((profileData.segments[0].stats.kd.value + Number.EPSILON) * 100)/100}}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Damage Done:
                  <span
                    class="stat-d"
                  >{{profileData.segments[0].stats.damageDone.displayValue}}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Healing Done:
                  <span
                    class="stat-d"
                  >{{profileData.segments[0].stats.healingDone.displayValue}}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Medals:
                  <span
                    class="stat-d"
                  >{{profileData.segments[0].stats.goldMedals.displayValue}} G/{{profileData.segments[0].stats.silverMedals.displayValue}} S/{{profileData.segments[0].stats.bronzeMedals.displayValue}} B</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Cards:
                  <span class="stat-d">{{profileData.segments[0].stats.cards.displayValue}}</span>
                </h3>
              </div>
            </div>
            <h5 class="w-100 text-center" style="font-size: 1.75rem;">
              <u>Bests</u>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Solo Eliminations:
                    <span
                      class="stat-d"
                    >{{profileData.segments[0].stats.mostSoloKills.displayValue}}</span>
                  </h3>
                </div>
              </div>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Damage Done:
                    <span
                      class="stat-d"
                    >{{profileData.segments[0].stats.mostDamageDone.displayValue}}</span>
                  </h3>
                </div>
              </div>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Healing Done:
                    <span
                      class="stat-d"
                    >{{profileData.segments[0].stats.mostHealingDone.displayValue}}</span>
                  </h3>
                </div>
              </div>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Time Spent On Fire:
                    <span
                      class="stat-d"
                    >{{profileData.segments[0].stats.mostTimeSpentOnFire.displayValue}}</span>
                  </h3>
                </div>
              </div>
            </h5>
          </div>
        </div>
        <div class="col-md-6 col-sm-12 col-xs-12">
          <div class="stats-comp-content ml-2 mr-2">
            <h5 class="w-100 pt-2" id="comp-title" style="text-align: center;">COMPETITIVE</h5>
            <hr style="background-color: #9dacbf; height: 3px;" />
            <h5 class="w-100 text-center" style="font-size: 1.75rem;">
              <u>Overall</u>
            </h5>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Win Rate:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[1].stats.wlPercentage.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Eliminations:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[1].stats.eliminations.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Deaths:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[1].stats.deaths.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  K/D Ratio:
                  <span class="stat-d">{{ profileData.segments[1].stats.kd.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Damage Done:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[1].stats.damageDone.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Healing Done:
                  <span
                    class="stat-d"
                  >{{ profileData.segments[1].stats.healingDone.displayValue }}</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Medals:
                  <span
                    class="stat-d"
                  >{{profileData.segments[1].stats.goldMedals.displayValue}} G/{{profileData.segments[1].stats.silverMedals.displayValue}} S/{{profileData.segments[1].stats.bronzeMedals.displayValue}} B</span>
                </h3>
              </div>
            </div>
            <div class="row">
              <div class="col-md-12 col-12">
                <h3 class="bottom-stats-title">
                  Cards:
                  <span class="stat-d">{{profileData.segments[1].stats.cards.displayValue}}</span>
                </h3>
              </div>
            </div>
            <h5 class="w-100 text-center" style="font-size: 1.75rem;">
              <u>Bests</u>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Solo Eliminations:
                    <span
                      class="stat-d"
                    >{{profileData.segments[1].stats.mostSoloKills.displayValue}}</span>
                  </h3>
                </div>
              </div>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Damage Done:
                    <span
                      class="stat-d"
                    >{{profileData.segments[1].stats.mostDamageDone.displayValue}}</span>
                  </h3>
                </div>
              </div>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Healing Done:
                    <span
                      class="stat-d"
                    >{{profileData.segments[1].stats.mostHealingDone.displayValue}}</span>
                  </h3>
                </div>
              </div>
              <div class="row">
                <div class="col-md-12 col-12">
                  <h3 class="bottom-stats-title">
                    Time Spent On Fire:
                    <span
                      class="stat-d"
                    >{{profileData.segments[1].stats.mostTimeSpentOnFire.displayValue}}</span>
                  </h3>
                </div>
              </div>
            </h5>
          </div>
        </div>
      </div>
    </div>
  </div>
</section>
</template>

<script>
import axios from "axios";
export default {
  name: "display",
  data() {
    return {
      loading: false,
      error: null,
      profileData: null,
    };
  },
  async created() {
    this.loading = true;
    try {
      const res = await axios.get(
        `/api/v1/profile/${this.$route.params.platform}/` +
          encodeURIComponent(`${this.$route.params.gamertag}`)
      );
      this.profileData = res.data.data;
      console.log(this.profileData);
      this.loading = false;
    } catch (err) {
      this.loading = false;
      this.error = err.response.data.message;
    }
  },
  methods: {
    convertTime(seconds) {
      let output = "";
      if (seconds > 86400) {
        output += Math.floor(seconds / 86400) + "D ";
        seconds %= 86400;
      }
      output += Math.floor(seconds / 3600) + "H ";
      seconds %= 3600;
      output += Math.floor(seconds / 60) + "M";
      return output;
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Raleway:wght@500;700&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap%27");
@import url("https://fonts.googleapis.com/css2?family=Teko&display=swap%27");
@import url("https://fonts.googleapis.com/css2?family=Russo+One&display=swap");
@import url("https://fonts.googleapis.com/css2?family=IBM+Plex+Serif:ital,wght@1,700&display=swap");
.error {
  position: relative;
  font-family: "Raleway", sans-serif;
}
.erbt {
  position: relative;
  display: flex;
  align-items: center;
}

.stat-d {
  font-family: "Russo One", "sans-serif";
  margin-left: 1.5rem;
}

#platform-logo {
  color: blue;
}

.stats-wrapper {
  background: rgba(0, 0, 0, 0.65);
  color: #cde1fa;
  float: center;
}
.error-box {
  font-family: "Russo One", "sans-serif";
  background: rgba(0, 0, 0, 0.65);
  color: #cde1fa;
  float: left;
  border-style: solid;
  border-color: #9dacbf;
  border-width: 3%;
  margin-top: 15%;
}
.stats-top-title {
  font-family: "Russo One", "sans-serif";
  background: rgba(0, 0, 0, 0.65);
  color: #cde1fa;
  float: center;
  border-style: solid;
  border-color: #9dacbf;
  border-width: 3%;
}

.stats-top-content {
  font-family: "Russo One", "sans-serif";
  background: rgba(0, 0, 0, 0.65);
  color: #cde1fa;
  float: center;
  border-style: solid;
  border-color: #9dacbf;
  border-width: 3%;
}

.stats-casual-content,
.stats-comp-content {
  font-family: "Russo One", "sans-serif";
  background: rgba(0, 0, 0, 0.65);
  color: #cde1fa;
  float: center;
  border-style: solid;
  border-color: #9dacbf;
  border-width: 3%;
}

.stats-title {
  font-size: 1.5rem;
  text-align: center;
  border-radius: 20px;
  align-items: center;
}

.bottom-stats-title {
  font-size: 1.5rem;
  text-align: left;
  margin-left: 1.5rem;
  margin-bottom: 2rem;
}

.stats-desc {
  font-size: 1.15rem;
  padding: 0.3rem 1.8rem;
  text-align: center;
  border-radius: 20px;
  margin-top: 0.2rem;
  margin-left: 0.3rem;
  align-items: center;
}

.gamertag {
  color: #ff9100;
  font-size: 3.5rem;
  padding: 0.3rem 0.5rem 0.3rem 1.5rem;
  text-align: center;
  display: flex;
  align-items: center;
  font-family: "Russo One", sans-serif;
  font-style: italic;
}

#casual-title,
#comp-title {
  font-family: "Russo One", "sans-serif";
  font-size: 2.5rem;
  text-align: center;
}

body {
  background-color: transparent;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  min-height: 100vh;
}
.preloader {
  position: relative;
  width: 100px;
  height: 70px;
  display: inline-block;
  transform-origin: center;
  animation: preloarder 1.5s linear infinite;
  margin-bottom: 10%;
}
.preloader .SpanLoad {
  display: flex;
  justify-content: center;
}
.preloader span {
  position: absolute;
  width: 15px;
  height: 15px;
  background: black;
  transform-origin: center 35px;
  border-radius: 50%;
}
.preloader span:first-child {
  opacity: 0.1;
}
.preloader span:nth-child(2) {
  transform: rotate(36deg);
  opacity: 0.2;
}
.preloader span:nth-child(3) {
  transform: rotate(72deg);
  opacity: 0.3;
}
.preloader span:nth-child(4) {
  transform: rotate(108deg);
  opacity: 0.4;
}
.preloader span:nth-child(5) {
  transform: rotate(144deg);
  opacity: 0.5;
}
.preloader span:nth-child(6) {
  transform: rotate(180deg);
  opacity: 0.6;
}
.preloader span:nth-child(7) {
  transform: rotate(216deg);
  opacity: 0.7;
}
.preloader span:nth-child(8) {
  transform: rotate(252deg);
  opacity: 0.8;
}
.preloader span:nth-child(9) {
  transform: rotate(288deg);
  opacity: 0.9;
}
.preloader span:nth-child(10) {
  transform: rotate(324deg);
  opacity: 1;
}

@keyframes preloarder {
  0% {
    transform: rotate(0deg) scale(1);
  }
  100% {
    transform: rotate(360deg) scale(1);
  }
}
</style>

