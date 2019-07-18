<template>
  <div id="dolist" class="text-center">
    <br />
    <h1 class="sitetitle">To Do List</h1>

    <div id="adding">
      <input
        id="new"
        type="textbox"
        :maxlength="maxchar"
        v-model="inputed"
        @keyup.enter="addList(inputed)"
      />
      <b-button variant="success" id="add" @click="addList(inputed)">+</b-button>
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

      <b-alert
        id="alert"
        v-model="showDismissibleAlert"
        variant="danger"
        dismissible
      >{{alertmessage}}</b-alert>

      <h4 class="sitetitle">List:</h4>
    </div>

  <div id="listing">
    <ul>
      <li class="outcontent" v-for="row in currentList" :key="row.id">
      <listing :getrow="row" :list="doList"></listing>
      
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
      selectList: []
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
      if (str.trim() !== "") {
        str = str.trim();

        if (
          this.doList.filter(v => v.title === str || str === "").length === 0
        ) {
          this.doList.push({
            id: this.randomKey(),
            title: str,
            status: "active"
          });
          this.showDismissibleAlert = false;
        } else if (str.length !== 0) {
          this.alertmessage = "Same Task Added!";
          this.showDismissibleAlert = true;
        } else {
          this.alertmessage = "Please, add text!";
          this.showDismissibleAlert = true;
        }

        this.inputed = "";
      } else {
        this.alertmessage = "Please, add text!";
        this.showDismissibleAlert = true;
        this.inputed = "";
      }
    }
  }
};
</script>

<style>
#new {
  float: inline-block;
  width: 38%;
  text-align: center;
  margin-right: 1%;
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
  float: inline-start;
}

input[type="textbox"] {
  padding: 5px;
  border-style: solid;
  border-width: 1px;
  border-color: rgb(34, 34, 34);
  border-radius: 10px;
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
</style>