<template>
  <nav class="nav">
      <a href="/">Home</a>
    <a v-for="nav in extraNav" :href="nav.href">{{ nav.desc }}</a>

    <transition-group tag="menu" name="nav__item" class="nav__menu">
      <li v-for="label in labels" class="nav__item" :key="label" @click="navBack">
        <i class="nav__item--icon"></i>
        <span class="nav__item--label">{{ label }}</span>
      </li>
    </transition-group>
  </nav>
</template>

<script>
export default {
  name: 'blog-nav',
  props: {
    navs: Number,
    content: Object,
    filters: {
      type: Object,
      default: () => {}
    }
  },

  computed: {
    labels() {
      return Object.keys(this.filters)
        .map(filter => this.content.labels[filter])
    },
    extraNav() {
      return window.vueextranav;
    }
  },

  methods: {
    navBack() {
      if (this.navs && !this.filters.author) this.$router.go(-1)
      else this.$router.push('/')
    }
  }
}
</script>
