<template>
  <div>
    <ul id="slide-out" class="sidenav sidenav-fixed">
      <li class="blue ">
        <router-link :to="{ name: 'Index' }">
          <img
            src="@/assets/images/shoryanLogo.png"
            alt=""
            class="shoryanLogo"
          />
        </router-link>
      </li>
      <li class="userSection">
        <div class="user-view">
          <div class="background">
            <img
              src="https://besthqwallpapers.com/Uploads/22-2-2018/41875/thumb2-material-design-gray-and-blue-lollipop-lines-art.jpg"
            />
          </div>
          <a href="#user">
            <img class="circle userAvatar" :src="userAvatarUrl" />
          </a>
          <a href="#name"
            ><span class="white-text name bold">{{
              this.$store.state.user.name
            }}</span></a
          >
          <a href="#email"
            ><span class="white-text email">{{
              this.$store.state.user.email
            }}</span></a
          >
          <router-link
            :to="{ name: 'EditProfile' }"
            class="btn-floating btn-medium waves-effect waves-light editProfile blue"
          >
            <i class="material-icons">edit</i>
          </router-link>
        </div>
      </li>
      <li>
        <router-link :to="{ name: 'Dashboard' }">
          <i class="material-icons">dashboard</i>Dashboard
        </router-link>
      </li>
      <li>
        <router-link :to="{ name: 'listUsers' }">
          <i class="material-icons">people</i>Users
        </router-link>
      </li>
      <li>
        <router-link :to="{ name: 'ListItems' }">
          <i class="material-icons">view_list</i>Listings
        </router-link>
      </li>
      <li>
        <router-link :to="{ name: 'listCategories' }">
          <i class="material-icons">library_books</i>Categories
        </router-link>
      </li>
      <li>
        <router-link :to="{ name: 'listDrugs' }">
          <i class="material-icons">view_list</i>Drugs
        </router-link>
      </li>
      <li>
        <router-link :to="{ name: 'Complaints' }">
          <i class="material-icons">warning</i>Complaints
        </router-link>
      </li>
      <li>
        <router-link :to="{ name: 'listGiftCards' }">
          <i class="material-icons">card_giftcard</i>Gift cards
        </router-link>
      </li>
    </ul>

    <div class="panelMainView">
      <nav class="white">
        <div class="container">
          <div class="nav-wrapper">
            <a href="#" data-target="slide-out" class="sidenav-trigger "
              ><i class="material-icons black-text">menu</i></a
            >
            <h5 class="black-text nomargin bold viewTitle">Dashboard</h5>
          </div>
        </div>
      </nav>
      <div class="container">
        <div class="row">
          <Notifications />
          <router-view> </router-view>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { EventBus } from "@/EventBus.js";

export default {
  mounted() {
    $(".sidenav").sidenav();
  },
  components: {
    Notifications: () => import("@/components/Notifications")
  },
  computed: {
    userAvatarUrl() {
      return this.$store.state.baseUrl + this.$store.state.user.img_url;
    }
  },
  beforeRouteUpdate(to, from, next) {
    EventBus.$emit("clearNotifications");
    next();
  }
};
</script>

<style lang="scss" scoped>
.shoryanLogo {
  height: 30px;
  vertical-align: middle;
}

.panelMainView {
  padding-left: 300px;
}

@media only screen and (max-width: 992px) {
  .panelMainView {
    padding-left: 0;
  }
}
nav {
  position: sticky;
  top: 0px;
  z-index: 997;
}
.sidenav li:first-of-type {
  padding: 8px 10px;
}
.userAvatar {
  object-fit: cover;
}
@media only screen and (min-width: 993px) {
  .container {
    width: 80%;
  }
}
.viewTitle {
  padding: 19px 0px;
}
.router-link-exact-active {
  background: #2196f3;
}
.background img {
  width: 100%;
}
.editProfile {
  width: 50px;
  height: 50px !important;
  line-height: 65px;
  position: absolute;
  right: 15px;
  top: 15px;
}
</style>
