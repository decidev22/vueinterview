<template>
  <div id="app">
    <div> 
      <!-- check all elements' detail 
            v-model <<<= v-model is essentially syntax sugar for updating data on user input events,
            plus special care for some edge cases.-->

      <!-- what is last-number for <<<= last number is for page -->

      <!-- why some in : and some not 
           total-rows and per-page is 
           total-rows = "rows"refer to the rows function
           -->
      <b-pagination
        v-model="currentPage"  
        :total-rows="rows"
        :per-page="perPage"
        last-number
      ></b-pagination>
    </div>
      <b-col lg="6">
        <!-- label col? sm? -->
        <b-form-group
          label="Filter"
          label-for="filter-input"
          label-cols-sm="3"
          label-align-sm="right"
          label-size="sm"
        >
          <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search OR choose a coloumn to search"
            ></b-form-input>
          </b-input-group>
        </b-form-group>

        <b-form-group
          label="Filter On"
          label-cols-sm="10"
          label-align-sm="right"
          label-size="sm"
          
        >
        <!-- :area-describedby ? was not needed-->
          <b-form-checkbox-group
            v-model="filterOn"
            
          >
            <b-form-checkbox value="Name">Name</b-form-checkbox>
            <b-form-checkbox value="Age">Age</b-form-checkbox>
            <b-form-checkbox value="Ticket">Ticket</b-form-checkbox>
            <b-form-checkbox value="Embarked">Embarked</b-form-checkbox>
            <b-form-checkbox value="class">PClass</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>
      </b-col>

      <!-- label-cols : how many columns the label should occupy in the row. 
          content-cols to decide how much width to take-->
      <b-form-group
          label="Per page"
          label-for="per-page-select"
          label-cols-sm='1'         
          label-align-sm="left"
          label-size="sm"
          content-cols-sm="1"
        >
          <b-form-select
            id="per-page-select"
            v-model="perPage"
            :options="pageOptions"
            size="sm"
            
          ></b-form-select>
      </b-form-group>
    <!-- @filtered ? what is @   <<<= is a decorator. simply a way of wrapping one piece of code with another
        Check if it works only with or even without it
        @filtered="onFiltered" 
        :fields="fields"
        works without-->
    <b-table striped hover :items="items"
            id="people-table"
            :per-page="perPage"
            :current-page="currentPage"
            
            :filter="filter"
            :filter-included-fields="filterOn"
            
            >

      <thead>
        <tr id ='table_top'>
          <th id='th1'>ID</th>
          <th id='th1'>Survived</th>
          <th id='th1'
            >PClass</th>
          <th id='th1'
            >Name</th>
          <th id='th1'>Sex</th>
          <th id='th1'
            >Age</th>
          <th id='th1'>Siblings</th>
          <th id='th1'>Parch</th>
          <th id='th1'
            >Ticket</th>
          <th id='th1'>Fare</th>
          <th id='th1'>Cabin</th>
          <th id='th1'>Embarked</th>
        </tr>
      </thead>

      <tr id ='data_table' v-for = "person in items" v-bind:key= "person.id">
        <!-- double {{}} meaning? -->
        <th id='th'>{{person.id}}</th>
        <th id='th'>{{person.Survived}}</th>
        <th id='th'>{{person.class}}</th>
        <th id='th'>{{person.Name}}</th>
        <th id='th'>{{person.Sex}}</th>
        <th id='th'>{{person.Age}}</th>
        <th id='th'>{{person.Siblings}}</th>
        <th id='th'>{{person.Parch}}</th>
        <th id='th'>{{person.Ticket}}</th>
        <th id='th'>{{person.Fare}}</th>
        <th id='th'>{{person.Cabin}}</th>
        <th id='th'>{{person.Embarked}}</th> 
      </tr>
    </b-table>
    <div>
      <!-- what is v-model -->
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        last-number
      ></b-pagination>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  data () {
    return {
      items: [],
      perPage: 20,
      currentPage: 1,
      filter: null,
      filterOn: [],
      //totalRows: 1,
      // fields: [
      //     { key: 'id'},
      //     { key: 'Survived'},
      //     { key: 'class', filterByFormatted: true},
      //     { key: 'Name', filterByFormatted: true},
      //     { key: 'Sex'},
      //     { key: 'Age', filterByFormatted: true},
      //     { key: 'Siblings'},
      //     { key: 'Parch'},
      //     { key: 'Ticket', filterByFormatted: true},
      //     { key: 'Fare'},
      //     { key: 'Cabin'},
      //     { key: 'Embarked', filterByFormatted: true}
      // ],
      pageOptions: [10, 20, 30, 40, 50, { value: 100, text: "Show a lot" }]
    }
  },

  computed: {
    rows() {
      return this.items.length //instead of having totalRows, I have put rows() in computed()
    }
  },

  // methods : {
  //   onFiltered(filteredItems) {
  //       this.totalRows = filteredItems.length // totalRows declared
  //       // this should have been on mounted() as well

  //       this.currentPage = 1
  //     }
  // },

  mounted () {
    axios
      .get('http://coding-task.strakertranslations.com/passengers')
      .then(response => (this.items = response.data))
    //this.totalRows = this.items.length
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
