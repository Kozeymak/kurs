<template>
    <div>
      <div class="pages">
        <v-btn v-on:click="sortNames">Отсортировать рецепты по алфавиту</v-btn>
      </div>
      <v-btn v-if="!isFives" v-on:click="filterByHp" >Отобразить только рецепты с ингридиентом яблоко</v-btn>
      <v-btn v-if="isFives" v-on:click="show" >Отобразить все</v-btn>
        <ul>
          <Order v-bind:heroes="heroes" />
        </ul>
      <div class="pages">
        <v-btn v-on:click="prev" >Предыдущая</v-btn>
        <v-btn v-on:click="next" >След страница</v-btn> 
      </div>
      <v-btn v-on:click="drop">Сбросить сортировку и фильтр</v-btn>
    </div>
</template>

<script>
import Order from '@/components/Order'

export default {
  data() {
    return {
      isFives: false
    }
  },
  components: {Order},
  async fetch({store}) {
    if (store.getters['her/recipes'].length === 0) {
      await store.dispatch('her/fetch')
    }
  },
  computed: {
    heroes() {
        console.log(this.$store.getters['her/recipes'])
        return this.$store.getters['her/recipes'];
    }
  },
  methods: {
    async show() {
      this.isFives = !this.isFives
      await this.$store.dispatch('her/fetch')
    },
    async filterByHp() {
      this.isFives = !this.isFives
      await this.$store.dispatch('her/filterByHp', this.$store.getters['her/recipes'])
    },
    async next() {
      const next = this.$store.getters['her/next'];
      if (next) {
        this.$store.dispatch('her/fetchNext', next);
      } else {
        alert('Следующей страницы нет')
      }
    },
    async prev() {
      const prev = this.$store.getters['her/prev'];
      if (prev) {
        this.$store.dispatch('her/fetchPrev', prev);
      } else {
        alert('Предыдущей страницы нет')
      }
    },
    async sortNames() {
      await this.$store.dispatch('her/fetchAll');
      const heroes = this.$store.getters['her/recipes'];
      await this.$store.dispatch('her/sortByNames', heroes);
    },
    async drop() {
      this.isFives = false
      await this.$store.dispatch('her/fetch')
    }
  }
}
</script>

<style>
  .pages {
    margin-top: 20px;
    margin-bottom: 20px;
  }
</style>