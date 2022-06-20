<script>
export default {
  name: "TableRow",
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
      <button type="button" class="btn me-4" v-on:click="removal(this.lineToDelete, true)">Yes</button>
      <button type="button" class="btn" v-on:click="removal(this.lineToDelete, false)">No</button>
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
          <tr v-for="(item, index) in rowItem" v-bind:key="item.title">
            <td><a v-bind:href="item.titleUrl" target="_blank">{{ item.title }}</a></td>
            <td>{{ item.author }}</td>
            <td>{{ item.quantity }}</td>
            <td>{{ item.isbn }}</td>
            <td>{{ item.date }}</td>
            <td>
              <button type="button" class="btn edit-btn me-4" v-on:click="passEdit(index)">Edit</button>
              <button type="button" class="btn-close btn-close-white btn-delete" aria-label="Close"
                v-on:click="deleteItem(index)"></button>
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

table>tbody>tr>td>button.btn-delete{
 box-sizing:border-box;
 width:1.5em;
}

.btn.edit-btn {
  color: #000;
}


table>tbody>tr>td>button.btn-delete {
  color: #fff;
  opacity: 1 !important;
  padding: none;
}

.eastTable table>tbody>tr>td>button.btn-delete {
  color: #000;
  filter: none;
}

.show-modal {
  display: block;
  width: 100%;
  height: 100%;
  background-image: radial-gradient(rgba(87,97,178,0.9),rgba(229, 180,222,0.9));
  padding: 1em;
}

.modal-inner{
  display: block;
  position: fixed;
  padding:3em;
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