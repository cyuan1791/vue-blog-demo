<template>
  <div class="col-xs-12">
  <ul class="nav nav-tabs">
    <li v-for="nav in extraNav"> <a :href="nav.href">{{ nav.desc }}</a></li>
  </ul>
    <div class="btn-group pull-right">
    <transition-group tag="ul" name="nav__item" class="nav nav-tabs">
      <button v-for="label in labels" class="btn btn-default" :key="label" @click="navBack">
       <a @click="navBack"> <span class="glyphicon glyphicon glyphicon-remove" aria-hidden="true"></span>
         {{ label }}</a>
      </button>
    </transition-group>
  </div>
  </div>
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
