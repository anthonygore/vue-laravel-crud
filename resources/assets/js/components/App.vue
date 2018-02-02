<template>
    <div class="container">
        <div class="row">
            <div class="col-md-8 col-md-offset-2">
                <div class="panel panel-default">
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
            </div>
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
        cruds: []
      }
    },
    methods: {
      create() {
        window.axios.get('/api/cruds/create').then(({ data }) => {
          this.cruds.push(new Crud(data));
        });
      },
      read() {
        window.axios.get('/api/cruds').then(({ data }) => {
          data.forEach(crud => {
            this.cruds.push(new Crud(crud));
          });
        });
      },
      update(id, color) {
        window.axios.put(`/api/cruds/${id}`, { id, color }).then(() => {
          this.cruds.find(crud => crud.id === id).color = color;
        });
      },
      del(id) {
        window.axios.delete(`/api/cruds/${id}`).then(() => {
          let index = this.cruds.findIndex(crud => crud.id === id);
          this.cruds.splice(index, 1);
        });
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
    .heading h1 {
        margin-bottom: 0;
    }
</style>
