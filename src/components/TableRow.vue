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
  methods: {
    deleteItem: function (index) {
      console.log(index);
      this.rowItem.splice(index, 1);
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
    <div id="table">
      <table class="table">
        <thead>
          <th scope="col" v-on:click="alphaOrder">Book Title</th>
          <th scope="col" v-on:click="alphaOrder">Author</th>
          <th scope="col">Quantity</th>
          <th scope="col">ISBN Number</th>
          <th scope="col" v-on:click="sortByDate(rowdata)">Date</th>
          <th scope="col">Change</th>
        </thead>
        <tbody>
          <!--  <TableRow v-for="(item, index) in tableInfo" v-bind:rowItem="item" v-bind:key="item.title"
            v-on:edit-me="editItem(index)" v-on:delete-me="deleteItem(index)" /> -->

          <tr v-for="(item, index) in rowItem" v-bind:rowItem="item" v-bind:key="item.title">
            <td><a v-bind:href="item.titleUrl" target="_blank">{{ item.title }}</a></td>
            <td>{{ item.author }}</td>
            <td>{{ item.quantity }}</td>
            <td>{{ item.isbn }}</td>
            <td>{{ item.date }}</td>
            <td>
              <button type="button" class="btn" v-on:click="passEdit(index)">Edit</button>
              <button type="button" class="close" aria-label="Close" v-on:click="deleteItem(index)"><span
                  aria-hidden="true"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor"
                    class="bi bi-x" viewBox="0 0 16 16">
                    <path
                      d="M4.646 4.646a.5.5 0 0 1 .708 0L8 7.293l2.646-2.647a.5.5 0 0 1 .708.708L8.707 8l2.647 2.646a.5.5 0 0 1-.708.708L8 8.707l-2.646 2.647a.5.5 0 0 1-.708-.708L7.293 8 4.646 5.354a.5.5 0 0 1 0-.708z" />
                  </svg></span></button>
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

.table td:nth-child(6) {
  display: flex;
  justify-content: space-between;
}
</style>