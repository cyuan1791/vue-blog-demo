<template>

  <div class="row">
    <div class="col-sm-12">
      <ul class="nav nav-tabs">
        <li v-for="nav in extraNav"> <a :href="nav.href">{{ nav.desc }}</a></li>
      </ul>
    </div>
    <div class="col-sm-12">
      <transition-group tag="div" name="nav" class="pull-right">
        <span v-for="label in labels" class="nav__item" :key="label" @click="navBack">
          <span class="glyphicon glyphicon-remove" aria-hidden="true"></span>
          <span class="nav__item--label">{{ label }}</span>
        </span>
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

