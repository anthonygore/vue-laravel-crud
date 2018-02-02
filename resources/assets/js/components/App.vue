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
  function Crud(id, color) {
    this.id = id;
    this.color = color;
  }

  let index = 0;

  let cruds = [
    new Crud(index, Math.random() < 0.5 ? 'red' : 'green'),
    new Crud(++index, Math.random() < 0.5 ? 'red' : 'green'),
  ];

  import CrudComponent from './Crud.vue';

  export default {
    data() {
      return {
        cruds
      }
    },
    methods: {
      create() {
        cruds.push(new Crud(++index, Math.random() < 0.5 ? 'red' : 'green'));
      },
      read() {

      },
      update(id, color) {
        console.log(id, color);
      },
      del(id) {
        console.log(id);
      }
    },
    components: {
      CrudComponent
    }
  }
</script>
<style>
    .heading h1 {
        margin-bottom: 0;
    }
</style>
