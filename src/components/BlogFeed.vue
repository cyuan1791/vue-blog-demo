<template>
  <transition-group tag="ul" :name="transition"  class="blog__feed">
    <li v-for="post in feed" class="preview" :key="post.id">
      <figure class="preview__figure" :class="figureClass" :style="getBgImg(post.image)">
        <transition name="v--fade">
          <figcaption v-if="!reading || $device.phone" class="preview__details">
            <router-link class="preview__title"
              :to="`/read/${post.id}`"
              @click.native="scrollTo(0, 220, scrollDelay)">
              {{ post.title }}
            </router-link>

            <div class="preview__meta">
              <time class="preview__published">
                {{ prettyDate(post.published) }}
              </time>

              <router-link class="preview__author"
                :to="`/by/${kebabify(post.author)}`"
                @click.native="scrollTo(0, 220, scrollDelay)">
                {{ post.author }}
              </router-link>
            </div>
          </figcaption>
        </transition>
      </figure>
    </li>
  </transition-group>
</template>

<script>
import { scrollTo, kebabify, prettyDate } from '../helpers'

export default {
  name: 'blog-feed',
  resource: 'BlogFeed',

  props: {
    filters: {
      type: Object,
      default: () => {}
    }
  },

  data() {
    return {
      posts: [],
      transition: 'preview-appear'
    }
  },

  computed: {
    reading() { return this.filters.post },
    scrollDelay() { return (this.$device.phone) ? 0 : 560 },
    figureClass() {
      return { 'preview__figure--mobile': this.$device.phone && this.reading }
    },
    feed() {
      const filterBy = {
        post: (filter, { id }) => filter === id,
        author: (filter, { author }) => filter === this.kebabify(author)
      }

      if (!Object.keys(this.filters).length) return this.posts

      return this.posts.filter(post => {
        return Object.keys(this.filters).every(filter => {
          return filterBy[filter](this.filters[filter], post)
        })
      })
    }
  },

  methods: {
    scrollTo,
    kebabify,
    prettyDate,
    getBgImg(src) {
      return { backgroundImage: `url(${src})` }
    },
    stackPosts(posts) {
      let interval
      const stack = () => {
        this.posts.push(posts.shift())

        if (!posts.length) {
          this.transition = 'preview'
          clearInterval(interval)
        }
      }

      interval = setInterval(stack, 125)
    }
  },

  beforeMount() {
    this.posts = [{
      'title': 'neque libero convallis eget',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/karl-magnuson.jpg',
      'published': '2017-09-14T18:17:00Z',
      'author': 'Michelle Blackford',
      'id': 'neque-libero-convallis-eget'
    }, {
      'title': 'sit amet erat',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/etienne-bosiger.jpg',
      'published': '2017-09-09T22:11:56Z',
      'author': 'Nathan Greave',
      'id': 'sit-amet-erat'
    }, {
      'title': 'posuere cubilia',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/saksham-gangwar.jpg',
      'published': '2017-09-05T00:55:44Z',
      'author': 'Hugo Curness',
      'id': 'posuere-cubilia'
    }, {
      'title': 'consequat ut nulla',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/mona-eendra.jpg',
      'published': '2017-07-28T18:31:01Z',
      'author': 'Derrik Yerrington',
      'id': 'consequat-ut-nulla'
    }, {
      'title': 'curabitur gravida nisi',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/adam-krowitz.jpg',
      'published': '2017-06-12T10:01:51Z',
      'author': 'Michelle Blackford',
      'id': 'curabitur-gravida-nisi'
    }, {
      'title': 'pretium nisl',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/steven-pahel.jpg',
      'published': '2017-06-02T03:22:11Z',
      'author': 'Nathan Greave',
      'id': 'pretium-nisl'
    }, {
      'title': 'vivamus tortor duis mattis',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/simone-hutsch.jpg',
      'published': '2017-05-27T00:05:26Z',
      'author': 'Tommi Filipson',
      'id': 'vivamus-tortor-duis-mattis'
    }, {
      'title': 'potenti cras in purus',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/casey-horner.jpg',
      'published': '2017-05-25T13:12:39Z',
      'author': 'Nathan Greave',
      'id': 'potenti-cras-in-purus'
    }, {
      'title': 'ultrices mattis odio',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/nate-rayfield.jpg',
      'published': '2017-05-10T13:57:35Z',
      'author': 'Nathan Greave',
      'id': 'ultrices-mattis-odio'
    }, {
      'title': 'nunc commodo placerat',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/kimon-maritz.jpg',
      'published': '2017-04-12T09:43:53Z',
      'author': 'Nathan Greave',
      'id': 'nunc-commodo-placerat'
    }, {
      'title': 'sit amet diam in',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/shalom-mwenesi.jpg',
      'published': '2017-04-03T22:21:22Z',
      'author': 'Derrik Yerrington',
      'id': 'sit-amet-diam-in'
    }, {
      'title': 'sapien ut nunc',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/robson-hatsukami-morgan.jpg',
      'published': '2017-03-10T21:11:06Z',
      'author': 'Derrik Yerrington',
      'id': 'sapien-ut-nunc'
    }, {
      'title': 'ut massa quis augue',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/tom-barrett.jpg',
      'published': '2016-12-15T23:29:56Z',
      'author': 'Nathan Greave',
      'id': 'ut-massa-quis-augue'
    }, {
      'title': 'lacinia eget tincidunt',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/joshua-hibbert.jpg',
      'published': '2016-11-02T02:21:18Z',
      'author': 'Martha Bonde',
      'id': 'lacinia-eget-tincidunt'
    }, {
      'title': 'dolor quis',
      'image': 'https://s3-us-west-2.amazonaws.com/s.cdpn.io/450744/maarten-van-den-heuvel.jpg',
      'published': '2016-10-27T09:42:32Z',
      'author': 'Tommi Filipson',
      'id': 'dolor-quis'
    }]
    // this.$getResource('feed')
      // .then(posts => {
      //   console.log(posts)
      //   if (!Object.keys(this.filters).length) {
      //     this.stackPosts(posts)
      //   } else {
      //     this.posts = posts
      //     this.transition = 'preview'
      //   }
      // })
  }
}
</script>
