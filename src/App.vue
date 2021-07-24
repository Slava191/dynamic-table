<template>

    <div class="fixed-top bg-secondary header">Динамическая таблица</div>
    <div class="container-fluid h-100">
        <div class="row h-100">
            <div class="col-2 sidebar bg-light">
                Sidebar
            </div>
            <div class="col-10 offset-2 main">
                <DynamicTable v-if="content" :contentProps="content" />
            </div>
        </div>
    </div>
    <div class="fixed-bottom bg-secondary footer">Footer</div>

</template>

<style>
    body, html {
        height: 100%;
    }

    .main{
        padding:100px 40px;
    }

    .sidebar {
        position: fixed;
        top: 80px;
        bottom: 0;
        padding: 30px;
    }


    .header, .footer {
        padding: 30px;
        color:white;
    }
</style>

<script>
import DynamicTable from './components/DynamicTable.vue'

export default {
  name: 'App',
  components: {
    DynamicTable
  },
  data(){
      return {
          content: null
      }
  },
  async created(){
      this.content = (await this.axios.get("https://city-mobil.ru/api/cars")).data
  }
}
</script>
