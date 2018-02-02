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
            <button @click="create()">Add</button>
        </div>
    </div>
</template>

<script>
  function Crud({ id, color, name}) {
    this.id = id;
    this.color = color;
    this.name = name;
  }

  import CrudComponent from './Crud.vue';

  export default {
    data() {
      return {
        cruds: [],
        working: false
      }
    },
    methods: {
      create() {
        this.working = true;
        window.axios.get('/api/cruds/create').then(({ data }) => {
          this.cruds.push(new Crud(data));
          this.working = false;
        });
      },
      read() {
        this.working = true;
        window.axios.get('/api/cruds').then(({ data }) => {
          data.forEach(crud => {
            this.cruds.push(new Crud(crud));
          });
          this.working = false;
        });
      },
      update(id, color) {
        this.working = true;
        window.axios.put(`/api/cruds/${id}`, { id, color }).then(() => {
          this.cruds.find(crud => crud.id === id).color = color;
          this.working = false;
        });
      },
      del(id) {
        this.working = true;
        window.axios.delete(`/api/cruds/${id}`).then(() => {
          let index = this.cruds.findIndex(crud => crud.id === id);
          this.cruds.splice(index, 1);
          this.working = false;
        });
      }
    },
    watch: {
      working(val) {
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
