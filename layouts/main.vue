<template>
  <div :style="theme.main">
    <el-menu
      v-if="!$store.state.authUser"
      class="nav"
      mode="horizontal"
      :background-color="theme.nav['backgroundColor']"
      :text-color="theme.nav['textColor']"
      :active-text-color="theme.nav['activeTextColor']"
      :default-active="$store.state.activeTab"
      @select="handleSelect"
    >
      <el-menu-item index="1">Intro</el-menu-item>
      <el-menu-item index="2" class="pRight">Register</el-menu-item>
    </el-menu>

    <el-menu
      v-else-if="$store.state.authUser.userStatus==1"
      class="nav"
      mode="horizontal"
      :background-color="theme.nav['backgroundColor']"
      :text-color="theme.nav['textColor']"
      :active-text-color="theme.nav['activeTextColor']"
      :default-active="$store.state.activeTab"
      @select="handleSelect"
    >
      <el-menu-item index="1">
        <nuxt-link to="/">Intro</nuxt-link>
      </el-menu-item>
      <el-menu-item index="2">Admin Panel</el-menu-item>
      <el-menu-item index="4" class="pRight">Logout</el-menu-item>
      <el-menu-item index="5" class="pRight">Settings</el-menu-item>
    </el-menu>

    <el-menu
      v-else
      class="nav"
      mode="horizontal"
      :background-color="theme.nav['backgroundColor']"
      :text-color="theme.nav['textColor']"
      :active-text-color="theme.nav['activeTextColor']"
      :default-active="$store.state.activeTab"
      @select="handleSelect"
    >
      <el-menu-item index="1">
        <nuxt-link to="/">Intro</nuxt-link>
      </el-menu-item>
      <el-menu-item index="2">
        <span>Dashboard</span>
        <el-badge style="margin-top:-2h; margin-left:0.5vw;" :value="$store.state.newUploadNotif"></el-badge>
      </el-menu-item>
      <el-menu-item index="3">Upload</el-menu-item>
      <el-menu-item index="4" class="pRight">Logout</el-menu-item>
      <el-menu-item index="5" class="pRight">Profile</el-menu-item>
    </el-menu>
    <div :style="theme.containerTheme" class="container">
      <nuxt />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {};
  },
  computed: {
    theme() {
      if (this.$store.state.settings.loaded) {
        return this.$store.state.settings.theme.data;
      } else {
        return {
          main: {
            position: "absolute",
            top: 0,
            left: 0,
            width: "100%",
            "min-height": "100%",
            background: "#f0f1e7"
          },
          nav: {
            backgroundColor: "#545c64",
            textColor: "#fff",
            activeTextColor: "#ffd04b"
          },
          containerTheme: { background: "#e6e7e8" }
        };
      }
    }
  },
  methods: {
    handleSelect(key, keyPath) {
      switch (key) {
        case "1":
          this.$nuxt._router.push("/");
          break;
        case "2":
          if (this.$store.state.authUser) {
            this.$nuxt._router.push("/dash");
          } else {
            this.$nuxt._router.push("/regg");
          }
          this.$store.dispatch("setActiveTab", "2");
          break;
        case "3":
          this.$nuxt._router.push("/upload");
          this.$store.dispatch("setActiveTab", "3");
          break;
        case "4":
          this.logout();
          break;
        case "5":
          if (this.$store.state.authUser.userStatus == 1) {
            this.$nuxt._router.push("/settings");
          } else {
            this.$nuxt._router.push("/profile");
          }
          this.$store.dispatch("setActiveTab", "5");
          break;
        default:
          console.log("invalid nav choice");
          break;
      }
    },
    async logout() {
      try {
        await this.$store.dispatch("logout");
        this.$nuxt._router.push("/");
      } catch (e) {
        this.formError = e.message;
      }
    }
  },
  created: function() {},
  head: {
    meta: [
      { property: "viewport", content: "width=device-width, initial-scale=1.0" }
    ]
  }
};
</script>



<style scoped>
.el-menu-item {
  font-size: 2.5vh;
  font-family: LatoLight;
}

.container {
  min-height: 100vh;
  max-width: 70vw;
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.pRight {
  float: right;
}

.nav {
  position: fixed;
  top: 0;
  left: 0;
  min-width: 100%;
  font-size: larger;
  z-index: 999;
}

nuxt-link {
  text-decoration: none;
}
</style>


<style>
h1 {
  font-weight: normal;
  -webkit-text-stroke: 0.2vmin;
}
h2,
h3,
h4,
h5 {
  font-weight: normal;
  -webkit-text-stroke: 0.05vmin;
}

@font-face {
  font-family: "LatoRegular";
  src: url("/fonts/LatoRegular/Lato-Regular.eot"),
    url("/fonts/LatoRegular/Lato-Regular.woff") format("woff"),
    url("/fonts/LatoRegular/Lato-Regular.ttf") format("truetype");
  font-style: normal;
  font-weight: normal;
}
@font-face {
  font-family: "LatoLight";
  src: url("/fonts/LatoLight/Lato-Light.eot"),
    url("/fonts/LatoLight/Lato-Light.woff") format("woff"),
    url("/fonts/LatoLight/Lato-Light.ttf") format("truetype");
  font-style: normal;
  font-weight: normal;
}
body {
  font-family: LatoRegular, sans-serif;
  overflow: scroll;
}

::-webkit-scrollbar {
  display: none;
}

a:link {
  color: #008fbf;
}
a:visited {
  color: #008fbf;
}
</style>

