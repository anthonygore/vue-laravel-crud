<template>
    <div id="app">
        <div class="heading">
            <h1>Cruds</h1>
        </div>
        <crud-component
                v-for="crud in cruds"
                v-bind="crud"
                :key="crud.id"
                @update="update"
                @delete="del"
        ></crud-component>
        <div>
            <button @click="create">Add</button>
        </div>
    </div>
</template>

<script>
  function Crud({ id, color, name}) {
    this.id = id;
    this.color = color;
    this.name = name;
  }

  import CrudComponent from './components/Crud.vue';

  export default {
    data() {
      return {
        cruds: [],
        mute: false
      }
    },
    methods: {
      async create() {
        this.mute = true;
        const { data } = await window.axios.get('/api/cruds/create');
        this.cruds.push(new Crud(data));
        this.mute = false;
      },
      async read() {
        this.mute = true;
        const { data } = await window.axios.get('/api/cruds');
        data.forEach(crud => this.cruds.push(new Crud(crud)));
        this.mute = false;
      },
      async update(id, color) {
        this.mute = true;
        await window.axios.put(`/api/cruds/${id}`, { color });
        this.cruds.find(crud => crud.id === id).color = color;
        this.mute = false;
      },
      async del(id) {
        this.mute = true;
        await window.axios.delete(`/api/cruds/${id}`);
        let index = this.cruds.findIndex(crud => crud.id === id);
        this.cruds.splice(index, 1);
        this.mute = false;
      }
    },
    watch: {
      mute(val) {
        document.getElementById('mute').className = val ? "on" : "";
      }
    },
    components: {
      CrudComponent
    },
    created() {
      this.read();
    }
  }
</script>
<style>
    #app {
        margin-left: 1em;
    }

    .heading h1 {
        margin-bottom: 0;
    }
</style>
