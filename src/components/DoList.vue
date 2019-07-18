<template>
  <div id="dolist" class="text-center">
    <br />

    <h1 class="sitetitle">To Do List</h1>

    <div id="adding">
      <input id="new" type="textbox" :maxlength="maxchar" v-model="inputed" @keyup.enter="addList(inputed)" />
      <b-button variant="success" id="add" @click="addList(inputed)">+</b-button>
    </div>

    <div id="changing">
      <div id="charactercount">Remaining Characters: {{remainingChar}}</div>
      <br>
      <label v-for="(tab, i) in tabs" :key="i">
        <div id="radioButtons">
          <input type="radio" name="tab" :checked="tab.status" @input="changeTab(i)" />
          {{ tab.title }}
        </div>
        <div id="radioButtonsCount">{{totalCount(tab.title.toLowerCase())}}</div>
      </label>

      <br />
      <br />

    <b-alert id="alert" v-model="showDismissibleAlert" variant="danger" dismissible>
      {{alertmessage}}
    </b-alert>

      <h4 class="sitetitle">List:</h4>
    </div>

    <div id="listing">
      <ul>
        <li class="outcontent" v-for="(row, index) in currentList" :key="row.id">
          <input
            type="checkbox"
            :id="row.id"
            v-model="row.status"
            true-value="completed"
            false-value="active"
          />
          <label class="todocontent" :for="row.id">{{ row.title }}</label>

          <b-button id="deletebutton" variant="danger" @click="delRow(index)">X</b-button>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        inputed: "",
        showDismissibleAlert: false,
        alertmessage: "",
        maxchar: 25,

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
        remainingChar: function(val){
          if(this.remainingChar===0){
            this.alertmessage = "Maximum Characters Reached.";
            this.showDismissibleAlert = true;
          }
          else{
            //this.showDismissibleAlert=false;
          }
        }
    },

    computed: {
      remainingChar(){
        return this.maxchar - this.inputed.length;
        
      },

      currentTab() {
        return this.tabs.find(tab => tab.status).title;
      },

      currentCounts() {
        let list = this.doList;

        switch (this.currentTab) {
          case "Active":
            list = this.doList.filter(todo => todo.status === "active");
            return todo.length;
            break;

          case "Completed":
            list = this.doList.filter(todo => todo.status === "completed");
            break;
        }
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
        console.log(str);
        if (str.trim() !== "") {
          str = str.trim();

          if (this.doList.filter(v => v.title === str || str === "").length === 0) {
            
            this.doList.push({
              id: this.randomKey(),
              title: str,
              status: "active"

            });
            this.showDismissibleAlert= false;
          }
          else if (str.length!==0){
            this.alertmessage = "Same Task Added!";
            this.showDismissibleAlert = true;
          }
          else{
            this.alertmessage = "Please, add text!";
            this.showDismissibleAlert = true;
          }

          this.inputed = "";
        }
        else{
            this.alertmessage = "Please, add text!";
            this.showDismissibleAlert = true;
             this.inputed = "";
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

  #new {
    float: inline-block;
    width: 38%;
    text-align: center;
    margin-right: 10px;
  }

  .sitetitle{  
    color:lightgray;
  }

   #charactercount {
    color:lightgray;
  }

  #adding {
    padding: 20px;
    padding-bottom: 0px;
  }

  #changing {
    margin: 20px;
  }

  #listing {
    display: inline-block;
    padding: 10px;
    width: 50%;
    height: 200px;
  }

  ul {
    list-style: none;
  }

  b-button {
    width: 40px;
    padding: 5px;

    text-align: center;
    background-color: brown;
    color: white;
    float: inline-start;
  }

  #add {
    float: inline-start;
  }

  input[type="checkbox"] {
    display: none;
  }

  input[type="textbox"] {
    padding: 5px;
    border-style: solid;
    border-width: 1px;
    border-color: rgb(34, 34, 34);
    border-radius: 10px;
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

  #deletebutton {
    font-size: 10px;
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

  #alert{
    display: inline-block;
    width: 40%;
  }


</style>
