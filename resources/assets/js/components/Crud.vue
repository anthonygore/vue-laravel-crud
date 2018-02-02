<template>
    <div class="crud">
        <div class="col-1">
            <img :src="image"/>
        </div>
        <div class="col-2">
            <h3>Name: {{ rand() | properCase }}</h3>
            <select @change="update">
                <option
                        v-for="col in [ 'red', 'green' ]"
                        :value="col"
                        :key="col"
                        :selected="col === color ? 'selected' : ''"
                >{{ col | properCase }}</option>

            </select>
            <button @click="del">Delete</button>
        </div>
    </div>
</template>
<script>
  export default {
    computed: {
      image() {
        return `/images/${this.color}.png`;
      }
    },
    methods: {
      update(val) {
        this.$emit('update', this.id, val.target.selectedOptions[0].value);
      },
      del() {
        this.$emit('delete', this.id);
      },
      rand() {
        let text = "";
        let possible = "abcdefghijklmnopqrstuvwxyz";

        for( let i=0; i < Math.random() * 10 ; i++ )
          text += possible.charAt(Math.floor(Math.random() * possible.length));

        return text;
      }
    },
    props: ['id', 'color'],
    filters: {
      properCase(string) {
        return string.charAt(0).toUpperCase() + string.slice(1);
      }
    }
  }
</script>
<style>
    .crud {
        display: flex;
        padding: 1em;
        border: 1px solid #d1d1d1;
        margin: 1em;
        max-width: 350px;
    }
    .crud img {
        height: 70px;
    }

    .col-2 {
        margin-left: 1em;
    }

    .col-2 > h3 {
        margin: 0.5em 0;
    }
</style>
