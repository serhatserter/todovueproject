<template>
  <div>
    <input type="checkbox" :id="rowlisting.id" v-model="rowlisting.status" true-value="completed" false-value="active"/>
    <label class="todocontent" :for="rowlisting.id">{{ rowlisting.title }}</label>
    <b-button id="deletebutton" variant="danger" @click="delRow(rowlisting.id)">X</b-button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      rowlisting: this.getrow,
      doList: this.list,
    };
  },

  props: {

    list: {
      type: Array,
      required: true
    },

    getrow: {
      required: true
    },

  },

  methods: {
    delRow(num) {
      let deleted = this.doList.find(tab => tab.id === num);
      var indx = this.doList.indexOf(deleted);

      this.doList.splice(indx, 1);
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
