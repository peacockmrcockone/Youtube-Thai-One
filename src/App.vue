<template>
  <nav>
    <router-link to="/" class="twod">
      {{ myanmarDate }} ရက် - {{ dayInMyanmar }}နေ့ {{ session }} ( ထိုင်း တိုက်ရိုက်၂ )
    </router-link> 
  </nav>
  <router-view/>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      dayInMyanmar: "",
      session: "", // မနက်ပိုင်း / ညနေပိုင်း
      myanmarDate: ""
    };
  },
  methods: {
    updateDay() {
      const daysMm = ["တနင်္ဂနွေ","တနင်္လာ","အင်္ဂါ","ဗုဒ္ဓဟူး","ကြာသပတေး","သောကြာ","စနေ"];
      const today = new Date().getDay();
      this.dayInMyanmar = daysMm[today];
    },
    updateDateTime() {
      const now = new Date();

      // Myanmar date format
      const options = { year: "numeric", month: "numeric", day: "numeric" };
      this.myanmarDate = new Intl.DateTimeFormat("my-MM", options).format(now);
    },
    updateSession() {
      const now = new Date();
      const hour = now.getHours();

      if (hour >= 10 && hour < 14) {
        this.session = "မနက်ပိုင်း";
      } else if (hour >= 14) {
        this.session = "ညနေပိုင်း";
      } else {
        this.session = "မနက်ပိုင်း";
      }
    }
  },
  mounted() {
    this.updateDay();
    this.updateDateTime();
    this.updateSession();

    // auto update every minute
    setInterval(() => {
      this.updateDateTime();
      this.updateSession();
    }, 60000);
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-bottom: 2px;
}

nav {
  padding: 10px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #ff0000;
  text-decoration: none;
  font-weight: bold;
}

.twod {
  font-size: 2.3rem;
}
</style>