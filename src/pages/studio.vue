<template lang="pug">
  .section: .container: .columns
    .column.is-one-quarter
      aside.menu
        ul.menu-list
          li: nuxt-link(:to="{name: 'studio-analytics'}")
            font-awesome-icon(:icon="['fas', 'analytics']" fixed-width)
            span.menu-text(v-t="'menu_analytics'")
          li: nuxt-link(:to="{name: 'studio-levels'}")
            font-awesome-icon(:icon="['fas', 'chart-network']" fixed-width)
            span.menu-text(v-t="'menu_levels'")
          li: nuxt-link(:to="{name: 'studio-collections'}")
            font-awesome-icon(:icon="['fas', 'archive']" fixed-width)
            span.menu-text(v-t="'menu_collections'")
          template(v-if="$store.state.user && ($store.state.user.role === 'moderator' || $store.state.user.role === 'admin')")
            p.menu-label Administration
            li: nuxt-link(:to="{name: 'studio-posts'}")
              font-awesome-icon(:icon="['fas', 'file-alt']" fixed-width)
              span.menu-text(v-t="'menu_posts'")
            li: nuxt-link(:to="{name: 'studio-users'}")
              font-awesome-icon(icon="users-cog" fixed-width)
              span.menu-text(v-t="'menu_users'")
            li: nuxt-link(:to="{name: 'studio-sudo'}")
              font-awesome-icon(icon="exchange" fixed-width)
              span.menu-text SUDO
    .column.is-three-quarters: nuxt-child
</template>

<script>
export default {
  name: 'Studio',
  middleware: 'auth',
  fetch ({ route, redirect }) {
    if (route.fullPath === '/studio') {
      redirect('/studio/levels')
    }
  },
  head () {
    const user = this.$store.state.user
    const name = user?.name || user?.uid || 'Unknown'
    return {
      title: name + "'s Studio - Cytoid"
    }
  },
  i18n: {
    key: 'studio'
  }
}
</script>

<style lang="scss" scoped>
.menu-text {
  font-size: 14px;
  font-weight: normal;
  margin-left: 8px;
}
a.is-active .menu-text {
  font-weight: bold;
}
.menu-list {
  li {
    margin-bottom: 8px;
  }
  a {
    padding: 12px 16px;
    color: rgba(255, 255, 255, 0.5);
    background-color: transparent;
    transition: 0.2s $hoverEasing;
    &:hover {
      color: rgba(255, 255, 255, 1);
      background-color: hsla(226, 15%, 19%, 0.3);
    }
  }
  a.is-active {
    color: rgba(255, 255, 255, 1);
    background: linear-gradient(to right, $theme4, $theme5);
    box-shadow: $ele4;
  }
}
</style>

<style lang="scss">
  .is-studio-table-thumbnail {
    width: 128px;
    height: 72px;
    img {
      width: 128px;
      height: 72px;
      transition: 0.2s $hoverEasing;
      &:hover {
        filter: brightness(50%);
      }
      border-radius: $radius;
      overflow: hidden;
      object-fit: cover;
    }
  }
</style>
