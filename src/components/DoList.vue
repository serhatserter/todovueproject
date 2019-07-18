<template>
  <div id="dolist" class="text-center">
    <br />
    <h1 class="sitetitle">To Do List</h1>

    <div id="adding">
      <b-input id="new" :maxlength="maxchar" v-model="inputed" @keyup.enter="addList(inputed)"/>
      <b-button variant="success" id="add" @click="addList(inputed)">Add Task</b-button>
    </div>

    <div id="changing">
      <div id="charactercount">Remaining Characters: {{remainingChar}}</div>
      <br />
      <label v-for="(tab, i) in tabs" :key="i">
        <div id="radioButtons">
          <input type="radio" name="tab" :checked="tab.status" @input="changeTab(i)" />
          {{ tab.title }}
        </div>
        <div id="radioButtonsCount">{{totalCount(tab.title.toLowerCase())}}</div>
      </label>

      <br />
      <br />

      <h4 class="sitetitle">List:</h4>
      <br />
      <b-alert
        id="alert"
        v-model="showDismissibleAlert"
        variant="danger"
        dismissible
      >{{alertmessage}}</b-alert>
    </div>

  <div id="listing">
    <ul>
      <li class="outcontent" v-for="row in currentList" :key="row.id">
        
        <listing :getrow="row" @delete="delRow(row.id)"></listing>
      
      </li>
    </ul>
  </div>

  </div>
</template>

<script>
import Listing from "./Listing.vue";
export default {
  components: { Listing },
  data() {
    return {
      inputed: "",
      showDismissibleAlert: false,
      alertmessage: "",
      maxchar: 25,
      deneme: {id: this.randomKey(), title: "Example Task", status: "active"},

      doList: [
        //{id: this.randomKey(), title: "Example Task", status: "active"}
      ],

      tabs: [
        {
          title: "All",
          status: true,
          count: 0
        },
        {
          title: "Active",
          status: false,
          count: 0
        },
        {
          title: "Completed",
          status: false,
          count: 0
        }
      ],
    };
  },

  watch: {
    remainingChar: function(val) {
      if (this.remainingChar === 0) {
        this.alertmessage = "Maximum Characters Reached.";
        this.showDismissibleAlert = true;
      }
    }
  },

  computed: {
    remainingChar() {
      return this.maxchar - this.inputed.length;
    },

    currentTab() {
      return this.tabs.find(tab => tab.status).title;
    },

    currentList() {
      let templist = this.doList;
      switch (this.currentTab) {
        case "Active":
          templist = this.doList.filter(todo => todo.status === "active");
          break;

        case "Completed":
          templist = this.doList.filter(todo => todo.status === "completed");
          break;
      }

      return templist;
    }
  },

  methods: {
    changeTab(tabIndex) {
      this.tabs.forEach(
        function(tab, i) {
          this.tabs[i].status = false;
        }.bind(this)
      );

      this.tabs[tabIndex].status = true;
    },

    totalCount(val) {
      if (val === "all") {
        return this.doList.length;
      }
      return this.doList.filter(todo => todo.status === val).length;
    },

    randomKey() {
      return Math.random()
        .toString(16)
        .slice(2);
    },

    addList(str) {

      if (!this.doList.find(v => v.title === str.trim()) && str.trim() !== "") {
          
        this.doList.push({id: this.randomKey(), title: str, status: "active"});
        this.showDismissibleAlert = false;
      } 
      else if (str.trim().length !== 0) {
        this.alertmessage = "Same Task Added!";
        this.showDismissibleAlert = true;
      } 
      else {
        this.alertmessage = "Please, add text!";
        this.showDismissibleAlert = true;
      }

      this.inputed = "";
      

    },

    delRow(num) {
      let deleted = this.doList.find(tab => tab.id === num);
      var indx = this.doList.indexOf(deleted);

      this.doList.splice(indx, 1);
    }
  }
};
</script>

<style>
#new {
  width: 38%;
  text-align: center;
  border-top-right-radius: 0;
  border-bottom-right-radius: 0;
}

.sitetitle {
  color: lightgray;
}

#charactercount {
  color: lightgray;
}

#adding {
  padding: 1%;
  padding-bottom: 0px;
}

#changing {
  margin: 1%;
}

ul {
  list-style: none;
}

b-button {
  width: 30%;
  padding: 5px;

  text-align: center;
  background-color: brown;
  color: white;
  float: inline-start;
}

#add {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
}

[type="text"] {
  padding: 5px;
  border-style: solid;
  border-width: 1px;
  border-color: rgb(34, 34, 34);
}

#radioButtons {
  display: block;
  padding: 10px;
  background-color: whitesmoke;
}

#radioButtons:hover {
  background-color: darkgray;
}

#radioButtons:checked {
  background-color: darkgray;
}

#radioButtonsCount {
  background-color: darkgray;
}

#alert {
  display: inline-block;
  width: 40%;
}

#listing {
  display: inline-block;
  padding: 1%;
  width: 50%;
  height: 200px;
}
.form-control {
    display: inline-block !important;
}
</style>