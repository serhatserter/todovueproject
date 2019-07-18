<template>
  <div>
    <input type="checkbox" :id="getrow.id" v-model="getrow.status" true-value="completed" false-value="active"/>
    <label class="todocontent" :for="getrow.id">{{ getrow.title }}</label>
    <b-button id="deletebutton" variant="danger" @click="getDeleted()">X</b-button>
  </div>
</template>

<script>
  export default {
    props: {

      getrow: {required: true},

    },

    methods: {
      getDeleted(){
          this.$emit('delete');
      }
    }
  };
</script>

<style>
  .todocontent {
    border-style: solid;
    border-width: 1px;
    border-color: rgb(34, 34, 34);
    border-radius: 10px;
    margin-right: 10px;
  }

  .todocontent:hover {
    background-color: brown;
    color: white;
  }

  #deletebutton {
    font-size: 10px;
  }

  input[type="checkbox"] {
    display: none;
  }

  input[type="checkbox"] + label {
    cursor: pointer;
    float: inline-start;
    width: 85%;
    padding: 1%;
    margin-left: -10px;
    background-color: whitesmoke;
  }

  input[type="checkbox"]:checked + label {
    background-color: darkgray;
    text-decoration: line-through;
  }
</style>
