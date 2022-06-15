<script>
/* import TableRow from "./TableRow.vue"; */


export default {
  name: "TableRow",
  /* components: {
    TableRow
  }, */
  props: {
    rowItem: {
      type: Object
    },
    tabInUse: {
      type: String
    }
  },
  data() {
    return {
      lineToDelete: '',
    }
  },
  methods: {
    deleteItem: function (index) {
      let deleteBox = document.getElementById("delete-box");
      deleteBox.classList.toggle("show-modal");

      console.log(index);

      return this.lineToDelete = index;
    },
    removal: function (line, option) {
      if (option === true) {
        this.rowItem.splice(line, 1);
      }
      let deleteBox = document.getElementById("delete-box");
      deleteBox.classList.toggle("show-modal");

    },
    passEdit: function (index) {
      this.$emit('editItem', {
        row: this.rowItem[index],
        spot: index
      })
    }
  }
}
</script>



<template>
  <div :class="['tab', { eastTable: tabInUse === 'East Side Shop' }]">
    <div class="modal" id="delete-box">
      <div class="modal-inner">
      <p><strong> Are you sure you want to delete this row?</strong></p>
      <hr>
      <button type="button" id="delete-yes" class="btn me-4" v-on:click="removal(this.lineToDelete, true)">Yes</button>
      <button type="button" id="delete-no" class="btn" v-on:click="removal(this.lineToDelete, false)">No</button>
      </div>
    </div>
    <div id="table">
      <table class="table">
        <thead>
          <tr>
            <th scope="col" v-on:click="alphaOrder">Book Title</th>
            <th scope="col" v-on:click="alphaOrder">Author</th>
            <th scope="col">Quantity</th>
            <th scope="col">ISBN Number</th>
            <th scope="col" v-on:click="sortByDate(rowdata)">Date</th>
            <th scope="col">Change</th>
          </tr>
        </thead>
        <tbody class="table-group-divider">
          <!--  <TableRow v-for="(item, index) in tableInfo" v-bind:rowItem="item" v-bind:key="item.title"
            v-on:edit-me="editItem(index)" v-on:delete-me="deleteItem(index)" /> -->

          <tr v-for="(item, index) in rowItem" v-bind:key="item.title">
            <td><a v-bind:href="item.titleUrl" target="_blank">{{ item.title }}</a></td>
            <td>{{ item.author }}</td>
            <td>{{ item.quantity }}</td>
            <td>{{ item.isbn }}</td>
            <td>{{ item.date }}</td>
            <td>
              <button type="button" class="btn edit-btn me-4" v-on:click="passEdit(index)">Edit</button>
              <button type="button" class="btn-close btn-close-white" aria-label="Close"
                v-on:click="deleteItem(index)"></button>
              <!--               <button type="button" class="close" aria-label="Close" v-on:click="deleteItem(index)"><span
                  aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                    class="bi bi-x" viewBox="0 0 16 16">
                    <path
                      d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z" />
                  </svg></span></button> -->
            </td>

          </tr>

        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.tab {
  background-color: #3d5485;
}

.eastTable {
  background-color: #e5b4de;
}

.table-group-divider {
  border-color: #fff;
}

.table thead th,
.table td {
  border-color: #f0f0f0;
  vertical-align: 0;
  color: #fff;
}

.eastTable .table thead th,
.eastTable .table td {
  color: #000;
}

.table td a {
  color: #fff;
  text-decoration: underline;
}

.eastTable .table td a {
  color: #000
}

.btn {
  background-color: #fff;
}

.btn.edit-btn {
  color: #000;
}

/* .table td:nth-child(6) {
  display: flex;
  justify-content: space-between;
} */

table>tbody>tr>td>button.btn-close {
  /* width:1.5em;
  height: 1.5em; */
  color: #fff;
  opacity: 1 !important;
  padding: none;
}

.eastTable table>tbody>tr>td>button.btn-close {
  color: #000;
  filter: none;
}

.show-modal {
  display: block;
 /*  top: 20%; */
 /*  left:25%; */
  width: 100%;
  height: 100%;
/*   background-color:rgba(148, 214, 243, 0.8); */
  background-image: radial-gradient(rgba(87,97,178,0.9),rgba(229, 180,222,0.9));
  padding: 1em;
  /* background-color: #fff;
  width:20em;
  height: auto;
  position: fixed; */
}

.modal-inner{
  display: block;
  position: fixed;
  padding:3em;
 /*  background-color: rgba(148, 214, 243, 0.5); */
  width:50%;
  left:25%;
  top:18%;
  text-align: center;
  font-size: 2em;
}


.modal-inner .btn{
font-size: 0.9em;  
}
</style>