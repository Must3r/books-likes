<template>
  <div id="app">
    <div class="container p-6">
      <h1 class="title is-size-1 has-text-white mb-5" v-text="title" />
      <ul class="contents has-text-left m-0">
        <li
          v-for="(chapter, index) in contents"
          :key="chapter.id"
          class="contents-item has-background-light px-5 py-3"
        >
          <span>{{ index + 1 }}. {{ chapter.text }}</span>
          <b-button
            type="is-light"
            @click="[like(chapter.id), likeDialog = true]"
          >
            <b-icon
              :class="[{ 'has-text-danger': isLiked(chapter.id) }, 'contents-item-like']"
              icon="heart"
            />
          </b-button>
        </li>
      </ul>
    </div>
    <b-modal v-model="likeDialog" :width="640" scroll="keep">
      <b-message v-if="isMessage">
        <p class="is-size-5">
          <span>Thanks for your review</span>
          <b-icon type="is-danger" icon="heart" size="is-medium" class="ml-3" />
        </p>
      </b-message>
      <div v-else class="card hearts">
        <div class="card-content">
          <div class="content">
            <p class="is-size-5 has-text-dark">Mark chapters you like most</p>
            <b-button
              v-for="chapter in contents"
              :key="`heart-${chapter.id}`"
              type="is-white"
              size="is-large"
              @click="like(chapter.id)"
            >
              <b-icon
                :class="[{ 'has-text-danger': isLiked(chapter.id) }, 'hearts-item']"
                icon="heart"
                size="is-large"
              />
            </b-button>
          </div>
          <p class="mb-5">
            <b-button type="is-dark" @click="send">
              Send
            </b-button>
          </p>
        </div>
      </div>
      <b-loading v-model="loading" />
    </b-modal>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data: () => ({
      title: 'Chapter 13',
      contents: [
        { id: 1, text: 'Crucifix gochujang scenester' },
        { id: 2, text: 'Squid shabby chic photo booth post-ironic' },
        { id: 3, text: 'La croix sartorial polaroid' },
        { id: 4, text: 'Listicle shabby chic subway tile' },
        { id: 5, text: 'Before they sold out etsy cardigan tumeric banjo blue bottle' },
        { id: 6, text: 'Helvetica before they sold out umami' },
        { id: 7, text: 'Banjo green juice fanny pack' },
        { id: 8, text: 'Next level adaptogen pabst bitters' },
        { id: 9, text: 'Chartreuse keytar chambray' },
        { id: 10, text: 'Cornhole church-key crucifix lyft' },
        { id: 11, text: 'Skateboard artisan direct trade irony vaporware occupy hexagon cold-pressed' },
        { id: 12, text: 'Humblebrag chicharrones farm-to-table iceland' },
        { id: 13, text: 'Post-ironic +1 vaporware leggings' }
      ],
      liked: [],
      likeDialog: false,
      loading: false,
      isMessage: false
    }),
    methods: {
      like(id) {
        if (!this.liked.includes(id)) {
          this.liked.push(id)
        } else {
          this.liked.splice(this.liked.indexOf(id), 1)
        }
      },
      isLiked(id) {
        return this.liked.includes(id)
      },
      send() {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.isMessage = true
          setTimeout(() => {
            this.likeDialog = false
            this.isMessage = false
          }, 2000)
        }, 3000)
      }
    }
  }
</script>

<style>
  body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    counter-reset: hearts;
  }
  #app {
    background-color: #232323;
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    flex: 1;
    text-align: center;
    color: #2c3e50;
  }
  .contents {
    border-radius: 4px;
    overflow: hidden;
  }
  .contents-item {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .contents-item:not(:last-child) {
    border-bottom: 1px solid lightgrey;
  }
  .contents-item-like {
    color: grey;
  }
  .hearts-item {
    position: relative;
    color: lightgrey;
  }
  .hearts-item::before {
    position: absolute;
    counter-increment: hearts;
    content: counter(hearts);
    font-size: 1rem;
    color: #232323;
    top: 50%;
    left: 50%;
    transform: translateX(-50%) translateY(-50%);
  }
  .hearts-item.has-text-danger::before {
    color: white;
  }
</style>