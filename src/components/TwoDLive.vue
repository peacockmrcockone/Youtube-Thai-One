<template>
  <!-- <div class="scroll-text">
      <p>
        📢 ခုလက်ရှိ ဏနန်း သည်  ❤️ {{ convertToMyanmarNumber(currentTwoD) }} ❤️ ဖြစ်ပါတယ်နော် 🔥
        🎓 EDUCATIONAL PURPOSE ONLY 
        🚫 NOT Gambling Advice / Promotion
        🔥 နေ့စဥ် ထိုင်း တိုက်ရိုက် Live တင်ဆက်ပေးနေတာဖြစ်လို့ 👍 LIKE & 🔔 SUBSCRIBE ပြုလုပ်ပေးခဲ့ပြီး အားပေးလို့ရပါတယ်. 
        📢 ခုလက်ရှိ ဏနန်း သည်  ❤️ {{ convertToMyanmarNumber(currentTwoD) }} ❤️ ဖြစ်ပါတယ်နော် 🔥 
      </p>
    </div> -->
  <div class="app">
    <div class="container">
      <!-- Right Panel -->
      <div class="right" v-if="twoDeeLive">
        <div class="phone">
          <h1 class="big-number">'{{ twoDeeLive.live.twod }}'</h1>
          <p class="updated">Updated: {{ updatedTime }}</p>
        </div>
        <div class="card">
          <h2>' 12:01 PM '</h2>
          <div class="info">
            <div class="info-item">
              <h3>' SET '</h3>
              <p>' {{ twoDeeLive.result[1].set }} '</p>
            </div>
            <div class="info-item">
              <h3>' Value '</h3>
              <p>' {{ twoDeeLive.result[1].value }} '</p>
            </div>
            <div class="info-item">
              <h3>' 2D '</h3>
              <p>' {{ twoDeeLive.result[1].twod }} '</p>
            </div>
          </div>
        </div>
        <div class="card">
          <h2>' 4:30 PM '</h2>
          <div class="info">
            <div class="info-item">
              <h3>' SET '</h3>
              <p>' {{ twoDeeLive.result[3].set }} '</p>
            </div>
            <div class="info-item">
              <h3>' Value '</h3>
              <p>' {{ twoDeeLive.result[3].value }} '</p>
            </div>
            <div class="info-item">
              <h3>' 2D '</h3>
              <p>' {{ twoDeeLive.result[3].twod }} '</p>
            </div>
          </div>
        </div>
      </div>
      <!-- Left Panel -->
      <div class="left" v-if="twoDeeLive">
        <ul>
          <li>🎓 EDUCATIONAL PURPOSE ONLY  </li>
          <li>📊 Market Data & Probability Learning </li>
          <li>🚫 NOT Gambling Advice / Promotion</li>
        </ul>
        <div class="time-box">
          <p class="label">ထွက်မည့် အချိန်  .......<span class="text-output">[" {{ convertToMyanmarNumber(currentTwoD) }} "]</span></p>
          <p class="main-time">{{ mainTime }}</p>
          <p class="countdown">{{ countdown }}</p>
        </div>

        <div class="date-row">
          <div class="date-box">{{ randomKey }} <span class="text">ဘရိတ်</span></div>
          <div class="num-box">{{ randomKey1 }} <span class="text">ကီး</span></div>
        </div>
        <!-- Footer -->
        <div class="subscribe">
          <button :class="{green:isGreen,red:!isGreen}">
            🔔 LIKE & SUBSCRIBE
          </button>
        </div>
      </div>
    </div>    
  </div>
</template>

<script>
export default {
  data() {
    return {
      today: "",
      updatedTime: "",
      countdown: "",
      randomKey: "",
      randomKey1: "",
      isGreen: false,
      twoDeeLive: null,
      error: "",
      isAnimated: false,
      previousLiveData: null,
      mainTime: "",
    };
  },
  computed: {
    // ✅ Auto switch between result[1] and result[3] by time
    currentTwoD() {
      if (!this.twoDeeLive) return "";
      const now = new Date();
      const cutoff = new Date();
      cutoff.setHours(14, 0, 0, 0); // 2:00 PM

      if (now < cutoff) {
        return this.twoDeeLive.result[1].twod;
      } else {
        return this.twoDeeLive.result[3].twod;
      }
    },
  },
  methods: {
    convertToMyanmarNumber(num){
      const myanmarNumbers = ["၀", "၁", "၂", "၃", "၄", "၅", "၆", "၇", "၈", "၉"];
      return num?.toString().split("").map(digit => myanmarNumbers[Number(digit)]).join("") || "";
    },


    updateDateTime() {
      const now = new Date();
      this.today = now.toLocaleDateString("en-GB");
      this.updatedTime = now.toLocaleString();
    },
    updateCountdown() {
      const now = new Date();

      // နာရီ/မိနစ်/စက္ကန့် ကိုယူမယ်
      const hours = String(now.getHours()).padStart(2, "0");
      const minutes = String(now.getMinutes()).padStart(2, "0");
      const seconds = String(now.getSeconds()).padStart(2, "0");

      // ပုံမှန်အတိုင်း တက်သွားမယ့် digital clock ပုံစံ
      this.countdown = `${hours}:${minutes}:${seconds}`;
    },
    generateUniqueKey() {
      let key;
      do {
        const temp = [];
        for (let i = 0; i < 2; i++) {
          temp.push(Math.floor(Math.random() * 10));
        }
        key = temp;
        // while condition = မလိုချင်တဲ့ pattern တွေကို စစ်
      } while (
        new Set(key).size < 2   // တူနေတဲ့ digit ရှိရင် reject
      );
      return key.join("-");
    },
    generateRandomKey() {
      this.randomKey = this.generateUniqueKey();
    },

    generateRandomKeyOne() {
      do {
        this.randomKey1 = this.generateUniqueKey();
      } while (this.randomKey1 === this.randomKey); // ဘရိတ်နဲ့တူမသွားအောင်
    },
    scheduleKeyUpdate() {
      const now = new Date();
      const morning = new Date();
      morning.setHours(10, 0, 0, 0);
      const afternoon = new Date();
      afternoon.setHours(14, 0, 0, 0);

      let nextUpdate;
      if (now < morning) {
        nextUpdate = morning;
      } else if (now < afternoon) {
        nextUpdate = afternoon;
      } else {
        nextUpdate = new Date();
        nextUpdate.setDate(now.getDate() + 1);
        nextUpdate.setHours(10, 0, 0, 0);
      }

      const diff = nextUpdate - now;
      setTimeout(() => {
        this.generateRandomKey();
        this.scheduleKeyUpdate();
      }, diff);
    },
    scheduleKeyUpdateOne() {
      const now = new Date();
      const morning = new Date();
      morning.setHours(10, 0, 0, 0);
      const afternoon = new Date();
      afternoon.setHours(14, 0, 0, 0);

      let nextUpdate;
      if (now < morning) {
        nextUpdate = morning;
      } else if (now < afternoon) {
        nextUpdate = afternoon;
      } else {
        nextUpdate = new Date();
        nextUpdate.setDate(now.getDate() + 1);
        nextUpdate.setHours(10, 0, 0, 0);
      }

      const diff = nextUpdate - now;
      setTimeout(() => {
        this.generateRandomKeyOne();
        this.scheduleKeyUpdateOne();
      }, diff);
    },
    async loadTwoDeeLive() {
      try {
        const response = await fetch("https://api.thaistock2d.com/live");
        const datas = await response.json();

        if (JSON.stringify(datas.live) !== JSON.stringify(this.previousLiveData)) {
          this.isAnimated = true;
          setTimeout(() => {
            this.isAnimated = false;
          }, 10000);
        }

        this.previousLiveData = datas.live;
        this.twoDeeLive = datas;
      } catch (err) {
        this.error = err.message;
      }
    },
    updateMainTime() {
      const now = new Date();
      const hour = now.getHours();
      const time12 = "12:01 PM";
      const time430 = "04:30 PM";

      if(hour >= 7 && hour < 14){
        this.mainTime = time12;
      } else {
        this.mainTime = time430;
      }
    },
  },
  mounted() {
    this.updateMainTime();
    this.updateDateTime();
    this.updateCountdown();
    this.generateRandomKey();
    this.scheduleKeyUpdate();
    this.generateRandomKeyOne();
    this.scheduleKeyUpdateOne();
    this.loadTwoDeeLive();
    setInterval(this.updateDateTime, 1000);
    setInterval(this.updateCountdown, 1000);
    setInterval(this.loadTwoDeeLive, 10000);
    setInterval(this.updateMainTime, 60000);
    setInterval(()=>{ this.isGreen = !this.isGreen; }, 2000);
  }
}
</script>


<style>
</style>




<style>
.app {
  font-family: sans-serif;
  background: #f0f0f0;
  padding: 20px;
}

.container {
  display: flex;
  gap: 10px;
  flex-wrap: nowrap;  /* ချုံ့တာနဲ့အောက် မကျအောင် */
}


.left,
.right {
  flex: 0 0 50%;   /* 50%-50% space equally */
  box-sizing: border-box;
}

/* left panel */
.left {
  background: white;
  padding: 15px;
  border-radius: 12px;
  gap: 30px;
}
.output-number{
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 30px;
}

/* right panel styled like iPhone frame */
.right {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #ffffff;
  border-radius: 40px;
  padding: 20px 15px;
  max-width: 440px;
  margin: auto;
  box-shadow: 0 10px 25px rgba(0,0,0,0.3);
  border: 8px solid #000;
  position: relative;
  width: 100%;
}

.right::before {
  content: "";
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 150px;
  height: 25px;
  background: #000;
  border-bottom-left-radius: 15px;
  border-bottom-right-radius: 15px;
}

/* inner content */
.phone {
  background: white;
  padding: 10px;
  width: 100%;
  max-width: 380px;
  margin-bottom: 15px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
}

.big-number {
  color: green;
  font-size: 9rem;
  text-align: center;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
  animation: float-text 2s steps(1) infinite;
}

.updated {
  text-align: center;
  font-size: 1.2rem;
  margin-bottom: 10px;
  font-weight: bold;
  color: #000000;
}

.card {
  width: 100%;
  max-width: 380px;
  background-color: #fc1818;
  color: white;
  border-radius: 10px;
  padding: 10px;
  margin-bottom: 7px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  justify-content: center;
  text-align: center;
  font-weight: bold;
}
.card h2 {
  font-size: 15px;
}

.card .info {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.info-item {
  text-align: center;
}

.info-item h3 {
  font-size: 1rem;
}

.info-item p {
  font-size: 14px;
}

.info-item:last-child p {
  color: yellow;
  font-size: 20px;
}


.result{
  font-size: 3.2rem;
  color: #ff0000;
}

.time-box {
  padding: 10px;
  border-radius: 12px;
  text-align: center;
  border: 2px solid #494949;
}
.label{
  font-size: 2.4rem;
  color: #ff0000;
  font-weight: bold;
}
.main-time {
  font-size: 3.1rem;
  font-weight: bold;
  color: #000000;
}

.countdown {
  font-size: 2.7rem;
  color: red;
  font-weight: 900;
}

.date-row {
  margin-top: 10px;
  display: flex;
  gap: 10px;
  font-weight: bold;
}
.date-box, .num-box {
  border: 2px solid #494949;
  padding: 15px;
  border-radius: 15px;
  flex: auto;
  color: #000000;
  font-size: 2.8rem;
  font-weight: bold;
  text-align: center;
}
/* subscribe */
.subscribe {
  margin-top: 30px;
  text-align: center;
  display: flex;
  justify-content: center;
  gap: 15px;   /* space between buttons */
  animation: float-text 4s ease-in-out infinite;
}

.subscribe button {
  color: white;
  padding: 10px 30px;
  font-size: 1.7rem;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  transition: background 0.3s ease;
}

.subscribe button.red { background: red; }
.subscribe button.green { background: green; }

.scroll-text {
  margin-top: 15px;
  overflow: hidden;
  white-space: nowrap;
  box-sizing: border-box;
  border-top: 2px solid #494949;
  border-bottom: 2px solid #494949;
  background: #ffffff;
}

.scroll-text p {
  animation: scroll-left 45s linear infinite; /* padding မသုံးတော့ */
  color: rgb(0, 0, 0);
  font-size: 1.5rem;
  font-weight: bold;
  margin: 0;
  padding: 5px 0;
}

@keyframes scroll-left {
  0% {
    transform: translateX(100%);   /* စ text ကို container ၏ ညာဘက်အပြင်ကနေ စ */
  }
  100% {
    transform: translateX(-100%);  /* လုံးဝ ဘယ်ဘက်အပြင်ထွက်သွား */
  }
}

/* Left panel list style */
.left ul {
  list-style: none;
  padding: 0;
  margin: 0 0 20px 0;
}

.left li {
  font-size: 1.8rem;
  font-weight: bold;
  margin-bottom: 10px;
  text-align: center;
  color: #000;
  animation: float-text 3s ease-in-out infinite;
}

/* တစ်ခုချင်းစီ interval အနည်းငယ်ကြာပြီးစလိုက်အောင် */
.left li:nth-child(1) {
  animation-delay: 0s;
}
.left li:nth-child(2) {
  animation-delay: 1s;
}
.left li:nth-child(3) {
  animation-delay: 2s;
}

/* left-right လှုပ် animation */
@keyframes float-text {
  0%, 100% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(30px);
  }
}
.text{
  font-size: 2.2rem;
}
.text-output{
  font-size: 3.5rem;
}

</style>