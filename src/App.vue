<template>
  <div class="root">
    <div @click="getState">
      保存
    </div>
    <aside>
      <div>
        <h3>模块</h3>
        <draggable :options="dragOptions">
          <div v-for="widget in widgets" :key="widget.name" class="widget-card" :type="widget.placeholder.type">
            <p>{{widget.name}}</p>
          </div>
        </draggable>
      </div>
    </aside>
    <main>
      <render :node="currentPage" :themeColor="site.config.color"/>
    </main>
  </div>
</template>

<script>
  require('./assets/iconfont.css');

  import { mapState, mapActions } from 'vuex'
  import Draggable from 'vuedraggable'

  import Render from './components/Render'

  export default {
    name: 'app',
    components: {
      Render,
      Draggable
    },
    data () {
      return {
        dragOptions: {
          group: {
            name: 'widgets',
            pull: 'clone',
            put: false
          },
          sort: false
        }
      }
    },
    computed: {
      ...mapState(['site', 'currentPage', 'widgets'])
    },
    methods: {
      ...mapActions(['getSite']),
      getState(){
        console.log(this.$store.state)
      }
    },
    async mounted () {
      await this.getSite()
    }
  }
</script>

<style>
  html, body {
    height: 100%;
  }
</style>

<style scoped>
  .root {
    height: 100%;
    display: flex;
    align-items: stretch;

  &>aside {
     width: 200px;
     border-right: 1px solid #ddd;

  & .widget-card {
      border: 1px solid;
      border-radius: 2px;
      width: 80px;
      display: flex;
      flex-direction: column;
      align-items: center;
      cursor: move;

  &>i {
     display: block;
     font-size: 2em;
   }
  &>p {
     margin-top: 5px;
     margin-bottom: 5px;
   }
  }
  }
  &>main {
     flex-grow: 1;
     padding: 0 20px;
   }
  }
</style>
