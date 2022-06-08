<template>
    <div id="app" class="container">
        <h1 class="text-center mb-5"> Book Nook Shop Inventory</h1>
        <div class="row">
            <div class="col-12">

                <form>
                    <div class="row">
                        <div class="col-6 mb-3" id="title_div">
                            <input v-model="title" type="text" class="form-control" placeholder="Enter Book Title"
                                id="title">
                        </div>
                        <div class="col-6" id="author_div">
                            <input v-model="author" type="text" class="form-control" placeholder="Author" id="author">
                        </div>
                        <div class="col-6 mb-3" id="quantity_div">
                            <input v-model.number="quantity" type="number" class="form-control"
                                placeholder="Quantity in Stock" id="quantity">
                        </div>
                        <div class="col-6" id="isbn_div">
                            <input v-model="isbn" type="text" class="form-control" placeholder="ISBN Number" id="isbn">
                        </div>
                        <div class="col-6" id="date_div">
                            <div class='input-group date' id='datetimepicker1'>
                                <input v-model="date" type="date" class="form-control" placeholder="Date">
                            </div>
                        </div>


                        <div class="col-6" id="button">
                            <div id="button_container_1" class="d-inline">
                                <button type="button" class="btn btn-primary add-btn"
                                    :class="['btn', 'btn-primary', 'add-btn', { affectingEast: currentData() === eastdata }]"
                                    v-on:click="addItem(currentData());" v-bind:disabled="isButtonDisabled">Add
                                    Row</button>
                            </div>
                            <div id="button_container_2" class="d-inline ml-3">
                                <button type="button"
                                    :class="['btn', 'btn-warning', 'update-btn', { affectingEast: currentData() === eastdata }]"
                                    v-show="showUpdate"
                                    v-on:click="updateItem(currentData(), lineToEdit)">Update</button>
                            </div>
                        </div>
                    </div>
                </form>
            </div>
            <div class="col-md-12 mt-5">


                <button v-for="tab in tabs" :key="tab"
                    :class="['tab-button', { active: currentTab === tab }, { eastTab: tab === 'East Side Shop' }, { westTab: tab === 'West Side Shop' }]"
                    @click="currentTab = tab">
                    {{ tab }}
                </button>
                <TableRow class="tab" v-bind:rowItem=currentData() @editItem="edit($event)"
                    v-bind:tabInUse="currentTab"></TableRow>
            </div>
        </div>
    </div>
</template>

<script>
import TableRow from "./components/TableRow.vue";


export default {
    name: 'app',
    components: { TableRow },
    data() {
        return {
            currentTab: 'West Side Shop',
            tabs: ['West Side Shop', 'East Side Shop'],
            title: '',
            author: '',
            date: '',
            quantity: '',
            isbn: '',
            isButtonDisabled: false,
            lineToEdit: '',
            showUpdate: false,
            westdata: [{ title: 'Harry Potter and the Sorcerer\'s Stone', titleUrl: 'https://www.google.com/search?q=Harry+Potter+and+the+Sorcerer%27s+Stone', author: ' J. K. Rowling', quantity: '10', isbn: '0399501487', date: '2022-05-30' },
            { title: 'Lord of the Flies', titleUrl: 'https://www.google.com/search?q=lord+of+the+flies', author: ' William Golding', quantity: '15', isbn: '0439708184', date: '2022-05-20' },
            { title: 'Roll of Thunder, Hear My Cry', titleUrl: 'https://www.google.com/search?q=roll+of+thunder+hear+my+cry', author: ' Mildred D. Taylor', quantity: '7', isbn: '0142401129', date: '2022-05-25' }],
            eastdata: [{ "title": 'Diary of a Wimpy Kid', "titleUrl": 'https://www.google.com/search?q=diary+of+a+wimpy+kid', "author": 'Jeff Kinney', "quantity": '5', "isbn": '1419741853', "date": '2022-05-02' },
            { title: 'The Hunger Games', titleUrl: 'https://www.google.com/search?q=the+hunger+games', author: 'Suzanne Collins', quantity: '8', isbn: '0439023483', date: '2022-05-08' }]
        }
    },
    methods: {
        alphaOrder: function (arr) {
            console.log('alpha');
            console.log(arr);
            arr.sort(function (a, b) {
                const nameA = a.title.toLowerCase();
                const nameB = b.title.toLowerCase();
                if (nameA < nameB) {
                    return -1;
                }
                if (nameA > nameB) {
                    return 1;
                }
                return 0;
            });
        },
        currentData() {
            if (this.currentTab == "West Side Shop") {
                return this.westdata;
            }
            return this.eastdata;

        },
        clearFields: function () {
            this.title = '';
            this.author = '';
            this.date = '';
            this.quantity = '';
            this.isbn = '';
        },
        addItem: function (arr) {
            console.log('title ' + this.title + typeof (this.title));
            if (!this.title) {
                alert("You must enter a title to add a row");
                return;
            }
            this.addingSortingClearing(arr);
        },

        addingSortingClearing: function (arr) {
            console.log('addsortclear');
            console.log(arr);
            this.pushIt(arr);
            this.alphaOrder(arr);
            this.clearFields();
        },

        pushIt: function (arr) {
            console.log('pushing');
            console.log(arr);

            var rowObject = {
                title: this.title,
                author: this.author,
                quantity: this.quantity,
                isbn: this.isbn,
                date: this.date,
                titleUrl: "https://www.google.com/search?q=" + this.title
            };
            console.log(rowObject);
            arr.push(rowObject);
            //check if page (key) already exists?
        },
        edit: function ({ row, spot }) {

            this.isButtonDisabled = true;
            this.showUpdate = true;


            this.title = row.title,
                this.author = row.author,
                this.quantity = row.quantity,
                this.isbn = row.isbn,
                this.date = row.data,
                this.titleUrl = row.titleUrl

            console.log(row);
            this.lineToEdit = spot;

            console.log(this.lineToEdit);
        },
        updateItem: function (arr, lineToEdit) {
            console.log(arr);
            console.log(lineToEdit);
            console.log(arr[lineToEdit]);

            var rowObject = {
                title: this.title,
                author: this.author,
                quantity: this.quantity,
                isbn: this.isbn,
                date: this.date,
                titleUrl: "https://www.google.com/search?q=" + this.title
            };

            if (!this.title) {
                alert("You must include a title in your edited row");
                return;
            }
            arr[lineToEdit] = rowObject;


            this.clearFields();

            this.isButtonDisabled = false;
            this.showUpdate = false;

        }

    }
}
</script>

<style>
body {
    background-color: #f0f0f0;
}

div.container {
    padding-top: 2em;
}

button.tab-button {
    width: 50%;
    padding: 1em;
    font-size: 1.3em;
    font-weight: 700;
    border-radius: 0.25em;
    border-color: transparent;
    border: solid 0.25em #d1d1d1;
    background-color: #e5b4de;
}

button.active:focus {

    outline: none;
}

button.westTab {
    background-color: #3d5485;
    border-color: #133376 !important;
    color: #fff;
}


button.eastTab {
    background-color: #e5b4de;
    border-color: #d166c2 !important;
}

/*comment */

.add-btn,
.update-btn {
    background-color: #3d5485;
    border-color: #3d5485;
    color: #fff;
}

.add-btn.affectingEast,
.update-btn.affectingEast {
    background-color: #e5b4de;
    border-color: #e5b4de;
    color: #000;
}
</style>