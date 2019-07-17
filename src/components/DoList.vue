<template>
  <div id="dolist">
    <center>
      <h1 id="sitetitle">To Do List</h1>
      <div id="adding">
        <input id="new" v-model="inputed" @keyup.enter="addList(inputed)" />

        <b-button variant="success" id="add" @click="addList(inputed)">+</b-button>
      </div>

      <div id="changing">
        <label v-for="(tab, i) in tabs" :key="i">
          <div id="radioButtons">
            <input type="radio" name="tab" :checked="tab.status" @input="changeTab(i)" />
            {{ tab.title }}
          </div>
        </label>
      </div>

      <div id="listing">
        <ul>
          <div class="outcontent" v-for="(row,index) in currentList" :key="index">
            <li>
              <input
                type="checkbox"
                :id="index"
                v-model="row.status"
                true-value="completed"
                false-value="active"
              />
              <label class="todocontent" :for="index">{{row.title}}</label>

              <b-button variant="danger" @click="delRow(index)">X</b-button>
            </li>
          </div>
        </ul>
      </div>
    </center>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputed: null,

      doList: [
        {
          title: "deneme",
          status: "active" //completed
        }
      ],
      tabs: [
        {
          title: "All",
          status: true
        },
        {
          title: "Active",
          status: false
        },
        {
          title: "Completed",
          status: false
        }
      ],
      selectList: []
    };
  },

  props: {},

  computed: {
    currentTab() {
      return this.tabs.find(tab => tab.status).title;
    },

    currentList() {
      let list = this.doList;

      switch (this.currentTab) {
        case "Active":
          list = this.doList.filter(todo => todo.status === "active");
          break;

        case "Completed":
          list = this.doList.filter(todo => todo.status === "completed");
          break;
      }

      return list;
    }
  },

  methods: {
    addList(str) {
      if (str !== null) {
        str = str.trim();
        console.log(str.trim());
        if (
          this.doList.filter(v => v.title === str || v.title === " " || str.trim() === "").length === 0) {
          this.doList.push({
            title: str,
            status: "active"
          });
        }
      }
    },

    delRow(num) {
      this.doList.splice(num, 1);
    },

    changeTab(tabIndex) {
      this.tabs.forEach(
        function(tab, i) {
          this.tabs[i].status = false;
        }.bind(this)
      );

      this.tabs[tabIndex].status = true;
    }
  }
};
</script>

<style>

#sitetitle{
  color: darkgray;
}

#adding {
  padding: 20px;
  padding-bottom: 0px;
}

#changing {
  margin: 20px;
}

ul {
  list-style: none;
}

b-button {
  width: 40px;
  padding: 10px;
  margin: 10px;
  text-align: center;
  background-color: brown;
  color: white;
}

input[type="checkbox"] {
  margin-top: 10px;
  width: 30px;
  height: 30px;
}
input[type="checkbox"] + label {
  float: inline-start;
  width: 250px;
  padding: 10px;
  text-align: center;
  background-color: whitesmoke;
}
input[type="checkbox"]:checked + label {
  background-color: darkgray;
  text-decoration: line-through;
}

#radioButtons {
  display: block;
  padding: 10px;
  background-color: whitesmoke;
}

#radioButtons:hover {
  background-color: darkgray;
}

.todocontent {
  margin-top: -100px;
}
</style>
