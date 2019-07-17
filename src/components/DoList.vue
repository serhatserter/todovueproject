<template>
  <div id="dolist">
    <center>
      <div id="adding">
        <input id="new" v-model="inputed" @keyup.enter="addList(inputed)" />
        <button id="add" @click="addList(inputed)">Add Button</button>
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
              <label :for="index">{{row.title}}</label>

              <label class="delete" @click="delRow(index)">X</label>
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
      this.doList.push({ title: str, status: "active" });
    },

    delRow(num) {
      this.doList.splice(num, 1);
      console.log(this.selectList);
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

.delete:hover {
  background-color: red;
}

.delete {
  width: 40px;
  padding: 10px;
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

input[type="radio"] {
  display: none;
}

#radioButtons {
  display: block;
  padding: 10px;
  background-color: whitesmoke;
}

#radioButtons:hover {
  background-color: darkgray;
}
</style>
