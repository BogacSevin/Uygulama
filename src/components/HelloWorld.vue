<template>
  <table id="myTable">
    <input
      type="text"
      id="myInput"
      v-model="filter"
      placeholder="Search for All.."
    />
     <input
      type="text"
      id="myInput1"
      v-model="filterCapital"
      @input="myFunction()"
      placeholder="Search for Capital.."
    />
    <tr class="header">
      <th scope="col">Country</th>
      <th scope="col">Capital</th>
      <th scope="col">Region</th>
      <th scope="col">Flag</th>
    </tr>

    <tr v-for="countrys in filteredTable" :key="countrys.id">
      <td>{{ countrys.name }}</td>
      <td>{{ countrys.capital }}</td>
      <td>{{ countrys.region }}</td>
     <td> <img v-bind:src="countrys.flag" width="80px" height="80px"> </td>
    </tr>
  </table>
</template>

<script>
import axios from "axios";
var _ = require('lodash');
export default {
  data() {
    return {
      country: null,
      filter: '',
      filterCapital: '',
    };
  },
  mounted() {
    //this.Api();
  },

  computed: {
    filteredTable() {
      return _.filter(this.country, (o) => { 
        if(this.filter == '') 
          return true;
        
        var rB = null;
        var rBfilters = [];

        var cS = null;
        var cSfilters =[];

        var lG = null;
        var lGfilters =[];

        var cR = null;
        var cRfilters = [];

       if(!_.isUndefined(o.currencies)) {
          cS = o.currencies;
          cSfilters = _.filter(o.currencies, (cSc) => {
            return cSc.code.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          })
        }
        if(!_.isUndefined(o.languages)) {
          lG = o.languages;
          lGfilters = _.filter(o.languages, (lGc) => {
            return lGc.name.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          })
        }
        if(!_.isUndefined(o.regionalBlocs)) {
          rB = o.regionalBlocs;
          rBfilters = _.filter(o.regionalBlocs, (rBc) => {
            return rBc.name.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          })
        }
        if(!_.isUndefined(o.currencies)) {
          cR = o.currencies;
          cRfilters = _.filter(o.currencies, (cRc) => {
            return cRc.name.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          })
        }
        //console.log(!_.isNull(rB) ? rBfilters.length > 0 : false);
        
        return (
          o.name.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          || o.region.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          || (
            o.capital ? o.capital.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0 : null
          )
          || 
           (  
            o.subregion ? o.subregion.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0 : null
          )
          || 
          (  
            o.demonym ? o.demonym.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0 : null
          )
          || ( !_.isNull(rB) ? rBfilters.length > 0 : false )
          || ( !_.isNull(cS) ? cSfilters.length > 0 : false )
          || ( !_.isNull(lG) ? lGfilters.length > 0 : false )
          || ( !_.isNull(cR) ? cRfilters.length > 0 : false )
        );
      });  
    },
  },
  created:function() {
    axios.get("https://restcountries.com/v2/all").then((res) => {
        this.country = res.data;
        //console.log(this.filteredTable);
      });
        },
  methods: {
    Api() {
      axios.get("https://restcountries.com/v2/all").then((res) => {
        this.country = res.data;
        //console.log(this.countrys);
      });
    },
    myFunction() {
      var table, tr, td, i, txtValue;
      table = document.getElementById("myTable");
      tr = table.getElementsByTagName("tr");
      for (i = 0; i < tr.length; i++) {
        td = tr[i].getElementsByTagName("td")[1];
        if (td) {
          txtValue = td.textContent || td.innerText;
          if (txtValue.toLocaleLowerCase('tr').includes(this.filterCapital.toLocaleLowerCase('tr'))) {
            tr[i].style.display = "";
          } else {
            tr[i].style.display = "none";
          }
        }
      }
    },
  },
};
</script>

<style>
#myInput {
  background-image: url("/css/searchicon.png"); /* Add a search icon to input */
  background-position: 10px 12px; /* Position the search icon */
  background-repeat: no-repeat; /* Do not repeat the icon image */
  width: 100%; /* Full-width */
  font-size: 16px; /* Increase font-size */
  padding: 12px 20px 12px 40px; /* Add some padding */
  border: 1px solid #ddd; /* Add a grey border */
  margin-bottom: 12px; /* Add some space below the input */
  float: left;
}
#myInput1 {
  background-image: url("/css/searchicon.png"); /* Add a search icon to input */
  background-position: 10px 12px; /* Position the search icon */
  background-repeat: no-repeat; /* Do not repeat the icon image */
  width: 100%; /* Full-width */
  font-size: 16px; /* Increase font-size */
  padding: 12px 20px 12px 40px; /* Add some padding */
  border: 1px solid #ddd; /* Add a grey border */
  margin-bottom: 12px; /* Add some space below the input */
  float: left;
  position: absolute;
  height: 50px;
  
}

#myTable {
  border-collapse: collapse; /* Collapse borders */
  width: 100%; /* Full-width */
  border: 1px solid #ddd; /* Add a grey border */
  font-size: 18px; /* Increase font-size */
}

#myTable th,
#myTable td {
  text-align: left; /* Left-align text */
  padding: 12px; /* Add padding */
}

#myTable tr {
  /* Add a bottom border to all table rows */
  border-bottom: 1px solid #ddd;
}

#myTable tr.header,
#myTable tr:hover {
  /* Add a grey background color to the table header and on hover */
  background-color: #f1f1f1;
}
</style>
<!-- -----
return _.filter(this.country, (o) => { 
 
        if(this.filter == o.regionalBlocs[0].name) 
        return true;
          return ( 
          o.name.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          || o.region.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0
          || (
            o.capital ? o.capital.toLocaleLowerCase('tr').indexOf(this.filter.toLocaleLowerCase('tr')) >= 0 : null
          )
        );
        
      });
-->