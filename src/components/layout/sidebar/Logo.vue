<template>
  <div class="sidebar-logo-container" :class="{'collapse':collapse}">
    <transition name="sidebar-logo-fade" mode="out-in">
      <router-link v-if="collapse" key="collapse" class="sidebar-logo-link" to="/">
        <img v-if="logo !== ''" :src="logo" class="sidebar-logo" alt="Sidebar Logo">
        <img v-else :src="require('@/assets/ClusterOperator-assist-white.png')" class="sidebar-logo" alt="Sidebar Logo">
      </router-link>
      <router-link v-else key="expand" class="sidebar-logo-link" to="/">
        <img v-if="logoWithText !== ''" :src="logoWithText" class="sidebar-logo" alt="Sidebar Logo">
        <img  v-else :src="require('@/assets/ClusterOperator-white.png')" class="sidebar-logo" alt="Sidebar Logo">
      </router-link>
    </transition>
  </div>
</template>

<script>
import store from '@/store'

export default {
  name: "SidebarLogo",
  props: {
    collapse: {
      type: Boolean,
      required: true,
    },
  },
  created() {
    if (store.getters.theme) {
      this.logoWithText = store.getters.theme.logoWithText
      this.logo = store.getters.theme.logo
    } else {
      this.$store.dispatch("theme/getThemeInfo").then((data) => {
        this.logoWithText = data.logoWithText
        this.logo = data.logo
      })
    }
  },
  data() {
    return {
      logoWithText: require("@/assets/ClusterOperator-white.png"),
      logo: require("@/assets/ClusterOperator-assist-white.png"),
    }
  },
}
</script>

<style lang="scss">
@import "~@/styles/common/variables";

.sidebar-logo-container {
  position: relative;
  height: $header-height;
  line-height: $header-height;
  overflow: hidden;

  &:after {
    content: "";
    position: absolute;
    bottom: 0;
    right: #{$sidebar-close-width / 4};
    height: 1px;
    width: calc(100% - #{$sidebar-close-width / 2});
    background-color: hsla(0, 0%, 100%, 0.5);
  }

  & .sidebar-logo-link {
    padding: 0 20px;
    display: flex;
    align-items: center;
    height: 100%;
    width: auto;

    & .sidebar-logo {
      height: $logo-height;
      vertical-align: middle;
    }
  }

  &.collapse {
    .sidebar-logo-link {
      padding: 0 10px;
    }

    .sidebar-logo {
      margin: auto;
    }
  }
}

.sidebar-logo-fade-enter-active {
  transition: opacity 0.1s;
  transition-delay: 0.1s;
}

.sidebar-logo-fade-leave-active {
  opacity: 0;
}

.sidebar-logo-fade-enter,
.sidebar-logo-fade-leave-to {
  opacity: 0;
}
</style>
