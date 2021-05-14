<template>
  <section>
    <h1 class="d-block mb-6">Characters</h1>
    <v-row class="sticky-top grey darken-3">
      <v-col cols="12" md="4">
        <v-text-field
          v-model="filter.name"
          label="Name"
          outlined
          clearable
          dense
          hide-details
          @input="resetPage"
        ></v-text-field>
      </v-col>
      <v-col cols="12" md="4">
        <v-select
          v-model="filter.status"
          :items="statuses"
          label="Status"
          dense
          outlined
          clearable
          hide-details
          @change="resetPage"
        ></v-select>
      </v-col>
      <v-col cols="12" md="4">
        <v-select
          v-model="filter.gender"
          :items="genders"
          label="Gender"
          dense
          outlined
          clearable
          hide-details
          @change="resetPage"
        ></v-select>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        v-for="character in characters"
        :key="character.id"
        cols="6"
        md="4"
        lg="3"
        xl="2"
      >
        <v-lazy>
          <v-card>
            <v-img :aspect-ratio="1" :src="character.image"></v-img>

            <div class="d-flex align-center">
              <div class="d-inline-flex flex-column overflow-hidden">
                <v-card-title class="d-block text-truncate">{{
                  character.name
                }}</v-card-title>
                <v-card-subtitle class="d-block text-truncate">
                  {{ character.species }}
                </v-card-subtitle>
              </div>
              <div
                class="
                  d-inline-flex
                  ml-auto
                  align-center
                  pa-4
                  justify-end
                  float-right
                "
                style="height: 100%"
              >
                {{ character.status }}
                <i
                  class="dot"
                  :style="{
                    background:
                      character.status === 'Alive'
                        ? 'green'
                        : character.status === 'Dead'
                        ? 'red'
                        : 'gray',
                  }"
                ></i>
              </div>
            </div>
          </v-card>
        </v-lazy>
      </v-col>
      <template v-if="loading"
        ><v-col
          v-for="i in 10"
          :key="`card-skeleton-${i}`"
          cols="6"
          md="4"
          lg="3"
          xl="2"
        >
          <v-skeleton-loader
            class="mx-auto"
            max-width="300"
            type="card"
          ></v-skeleton-loader> </v-col
      ></template>
    </v-row>
    <div v-if="info.pages <= filter.page" class="text-center my-4">
      No more characters
    </div>
    <div v-if="error.length" class="text-center my-4">{{ error }}</div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      characters: [],
      maxPages: 2,
      loading: true,
      error: '',
      timeout: {},
      info: {},
      filter: {
        page: 1,
        name: '',
        gender: '',
        status: '',
      },
      genders: [
        {
          text: 'Female',
          value: 'Female',
        },
        {
          text: 'Male',
          value: 'Male',
        },
        {
          text: 'Genderless',
          value: 'Genderless',
        },
        {
          text: 'Unknown',
          value: 'unknown',
        },
      ],
      statuses: [
        {
          text: 'Alive',
          value: 'Alive',
        },
        {
          text: 'Dead',
          value: 'Dead',
        },
        {
          text: 'unknown',
          value: 'unknown',
        },
      ],
    }
  },
  fetch() {
    this.getCharacters()
  },
  watch: {
    filter: {
      handler(val, oldVal) {
        if (this.filter.page <= this.info.pages) this.getCharacters()
      },
      deep: true,
    },
  },
  mounted() {
    this.scrollHandler()
  },
  methods: {
    async getCharacters() {
      this.loading = true
      this.error = ''
      try {
        const filter = Object.fromEntries(
          Object.entries(this.filter).filter(([_, v]) => v != null)
        )
        const query = new URLSearchParams(filter).toString()
        const { data } = await this.$axios.get(
          `https://rickandmortyapi.com/api/character/?${query}`
        )
        this.info = data.info

        if (this.filter.page === 1) {
          this.characters = data.results
        } else {
          this.characters.push(...data.results)
        }
      } catch (e) {
        this.characters = []
        this.info = {}
        this.error = e.response.data.error
      }
      this.loading = false
    },
    scrollHandler(e) {
      window.onscroll = () => {
        const bottomOfWindow =
          Math.max(
            window.pageYOffset,
            document.documentElement.scrollTop,
            document.body.scrollTop
          ) +
            window.innerHeight >
          document.documentElement.offsetHeight - 300

        if (bottomOfWindow) {
          clearTimeout(this.timeout)
          this.timeout = setTimeout(() => {
            this.filter.page++
          }, 300)
        }
      }
    },
    resetPage() {
      this.info.pages = 1
      this.filter.page = 1
    },
  },
}
</script>

<style>
@media (min-width: 960px) {
  .sticky-top {
    position: sticky;
    top: 64px;
    z-index: 10;
  }
}

.dot {
  width: 10px;
  height: 10px;
  border-radius: 50%;
  margin-left: 12px;
}
</style>
