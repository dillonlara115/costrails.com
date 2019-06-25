<template>
   <div>
      <div class="field">
         <div class="control">
            <input type="text" class="input" v-model="search" placeholder="search trails" />
         </div>
      </div>
    <table class="table is-hoverable is-striped is-fullwidth">
      <thead>
       <th v-for="column in columns">
						{{column.label}}
						
					</th>
      </thead>
      <tr v-for="condition in filteredConditions" >
        <td>{{ condition.name }}</td>
        <td>
          <span v-if="condition.conditionColor == 'Green'">
            <i class="fas fa-thumbs-up has-text-primary is-size-4"></i>
          </span>
          <span v-else-if="condition.conditionColor == 'Yellow'">
            <i class="fas fa-hand-rock has-text-warning is-size-4"></i>
          </span>
          <span v-else-if="condition.conditionColor == 'Red'">
            <i class="fas fa-hand-middle-finger has-text-danger is-size-4"></i>
          </span>
          <br>
          {{ condition.conditionStatus }}
          <br>
          <small>{{ condition.conditionDetails }}</small>
        </td>
        <td>
          <span
            v-if="condition.conditionDate"
          >{{getConditionDate(condition.conditionDate)}}</span><br>
          <span
            v-if="condition.urlConditionsUpdate"
          >
         <a :href="condition.urlConditionsUpdate" target="_blank"  class="button is-light is-fullwidth">update</a></span>
        </td>
      </tr>
    </table>
   </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
export default {
   name: "app",
   components: {
      
   },
   data() {
      return {
         conditions: [],
         search: "",
         columns: [
            {label: 'Trail', shortcode: 'trail'},
           { label: 'Condition', shortcode: 'condition'},
           { label: 'Last Updated', shortcode: 'updated'}
         ],
         currentSort:'trail',
         currentSortDir:'asc'
      };
   },
   methods: {
      getConditionDate : function (date) {

         return moment(date, 'YYYY-MM-DD').format('MMMM DD');

      },
      sort:function(col) {
      // if you click the same label twice
      if(this.currentSort == col){
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      }else{
        this.currentSort = col;
      } // end if

    }, // sort
   },
   mounted() {
      axios
         .get(
            "https://www.mtbproject.com/data/get-conditions?ids=7044665,7038543,3504412,4307089,7041274,7037093,270021,7020372,789594,712638,7033173,2214920&key=200338445-e5ebb8bb1a9a7f8e6686296dab783b1e"
         )
         .then(response => (this.conditions = response.data));
   },
   computed: {
      filteredConditions() {
         let listOfObjects = Object.keys(this.conditions).map((key) => {
            return this.conditions[key]
         })
         listOfObjects.splice(-1,1) // removes success entry from api response
         return listOfObjects.filter((condition) => {
            return condition.name.toLowerCase().match(this.search);
         }).sort((a, b) => {
        if (this.currentSortDir === 'asc') {
	        return a[this.currentSort] >= b[this.currentSort];      
        }
        return a[this.currentSort] <= b[this.currentSort];
      });
      }
   }
};
</script>