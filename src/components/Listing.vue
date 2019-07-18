<template>
  <div id="listing">
    <ul>
      <li class="outcontent" v-for="row in currentList" :key="row.id">
        <input
          type="checkbox"
          :id="row.id"
          v-model="row.status"
          true-value="completed"
          false-value="active"
        />
        <label class="todocontent" :for="row.id">{{ row.title }}</label>

        <b-button id="deletebutton" variant="danger" @click="delRow(row.id)">X</b-button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      doList: this.list,
      tabs: this.listtabs
    };
  },

  props: {
    list: {
      type: Array,
      required: true
    },

    listtabs: {
      type: Array,
      required: true
    }
  },

  computed: {
    currentTab() {
      return this.tabs.find(tab => tab.status).title;
    },

    currentList() {
      let templist = this.list;
      switch (this.currentTab) {
        case "Active":
          templist = this.list.filter(todo => todo.status === "active");
          break;

        case "Completed":
          templist = this.list.filter(todo => todo.status === "completed");
          break;
      }

      return templist;
    }
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
#listing {
  display: inline-block;
  padding: 10px;
  width: 50%;
  height: 200px;
}
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
  padding: 10px;
  margin-left: -10px;
  background-color: whitesmoke;
}

input[type="checkbox"]:checked + label {
  background-color: darkgray;
  text-decoration: line-through;
}
</style>
