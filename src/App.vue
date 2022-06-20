<template>
    <div id="app" class="container">
        <h1 class="text-center mb-5"> Book Nook Shop Inventory</h1>
        <div class="row">
            <div class="col-12">

                <form :class="{ affectingEast: currentData() === eastdata }">
                    <div class="row">
                        <div class="col-6 mb-3">
                            <input v-model="title" type="text" class="form-control form-inv"
                                placeholder="Enter Book Title">
                        </div>
                        <div class="col-6">
                            <input v-model="author" type="text" class="form-control form-inv" placeholder="Author">
                        </div>
                        <div class="col-6 mb-3">
                            <input v-model.number="quantity" type="number" class="form-control form-inv"
                                placeholder="Quantity in Stock">
                        </div>
                        <div class="col-6">
                            <input v-model="isbn" type="text" class="form-control form-inv" placeholder="ISBN Number">
                        </div>
                        <div class="col-6">
                            <div class='input-group date'>
                                <input v-model="date" type="date" class="form-control form-inv" placeholder="Date">
                            </div>
                        </div>


                        <div class="col-6">
                            <div class="d-inline">
                                <button type="button" class="btn btn-primary add-btn"
                                    :class="['btn', 'btn-primary', 'add-btn', { affectingEast: currentData() === eastdata }]"
                                    v-on:click="addItem(currentData());" v-bind:disabled="isButtonDisabled">Add
                                    Row</button>
                            </div>
                            <div class="d-inline ms-3">
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
            { title: 'Lord of the Flies', titleUrl: 'https://www.google.com/search?q=lord+of+the+flies', author: ' William Golding', quantity: '15', isbn: '978-0571295715', date: '2022-05-20' },
            { title: 'Roll of Thunder, Hear My Cry', titleUrl: 'https://www.google.com/search?q=roll+of+thunder+hear+my+cry', author: ' Mildred D. Taylor', quantity: '7', isbn: '0142401129', date: '2022-05-25' }],
            eastdata: [{ "title": 'Diary of a Wimpy Kid', "titleUrl": 'https://www.google.com/search?q=diary+of+a+wimpy+kid', "author": 'Jeff Kinney', "quantity": '5', "isbn": '1419741853', "date": '2022-05-02' },
            { title: 'The Hunger Games', titleUrl: 'https://www.google.com/search?q=the+hunger+games', author: 'Suzanne Collins', quantity: '8', isbn: '978-1338321913', date: '2022-05-08' }]
        }
    },
    methods: {
        alphaOrder: function (arr) {
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
            if (!this.title) {
                alert("You must enter a title to add a row");
                return;
            }
            /* console.log(arr.map(entry => entry.title)); */
            let match = arr.map(entry => entry.title).includes(this.title);
           /*  console.log(match); */
            if(match){
                alert("You must enter a unique title to add a row");
                return;
            }
            if (this.isbn) {
                this.checkISBN();
            }
            this.addingSortingClearing(arr);
        },

        addingSortingClearing: function (arr) {
            this.pushIt(arr);
            this.alphaOrder(arr);
            this.clearFields();
        },

        checkISBN: function () {

            var subject = this.isbn;

            // Checks for ISBN-10 or ISBN-13 format
            var regex = /^(?:ISBN(?:-1[03])?:? )?(?=[0-9X]{10}$|(?=(?:[0-9]+[- ]){3})[- 0-9X]{13}$|97[89][0-9]{10}$|(?=(?:[0-9]+[- ]){1,4})[- 0-9]{14,17}$)(?:97[89][- ]?)?[0-9]{1,5}[- ]?[0-9]+[- ]?[0-9]+[- ]?[0-9X]$/;

            if (regex.test(subject)) {
                // Remove non ISBN digits, then split into an array
                var chars = subject.replace(/[- ]|^ISBN(?:-1[03])?:?/g, "").split("");
                // Remove the final ISBN digit from `chars`, and assign it to `last`
                var last = chars.pop();
                var sum = 0;
                var check, i;

                if (chars.length == 9) {
                    // Compute the ISBN-10 check digit
                    chars.reverse();
                    for (i = 0; i < chars.length; i++) {
                        sum += (i + 2) * parseInt(chars[i], 10);
                    }
                    check = 11 - (sum % 11);
                    if (check == 10) {
                        check = "X";
                    } else if (check == 11) {
                        check = "0";
                    }
                } else {
                    // Compute the ISBN-13 check digit
                    for (i = 0; i < chars.length; i++) {
                        sum += (i % 2 * 2 + 1) * parseInt(chars[i], 10);
                    }
                    check = 10 - (sum % 10);
                    if (check == 10) {
                        check = "0";
                    }
                }

                if (check == last) {
                    /* alert("Valid ISBN"); */
                    return this.isbn;
                } else {
                    alert("Invalid ISBN check digit. Your entry will be made without the ISBN.");
                    return this.isbn = "";
                }
            } else {
                alert("Invalid ISBN. Your entry will be made without the ISBN.");
                return this.isbn = "";
            }
        },

        pushIt: function (arr) {
            var rowObject = {
                title: this.title,
                author: this.author,
                quantity: this.quantity,
                isbn: this.isbn,
                date: this.date,
                titleUrl: "https://www.google.com/search?q=" + this.title
            };
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
                this.date = row.date,
                this.titleUrl = row.titleUrl

            this.lineToEdit = spot;
        },
        updateItem: function (arr, lineToEdit) {

            if (!this.title) {
                alert("You must include a title in your edited row");
                return;
            }
            if (this.isbn) {
                this.checkISBN();
            }
            var rowObject = {
                title: this.title,
                author: this.author,
                quantity: this.quantity,
                isbn: this.isbn,
                date: this.date,
                titleUrl: "https://www.google.com/search?q=" + this.title
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

input.form-inv:focus {
    box-shadow: 0 0 0 0.25rem rgb(61 84 133 / 25%);
    border-color: #3d5485;
}

form.affectingEast input.form-inv:focus {
    box-shadow: 0 0 0 0.25rem rgb(229 180 222 / 25%);
    border-color: #e5b4de;
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

.add-btn,
.add-btn:disabled,
.update-btn {
    background-color: #3d5485;
    border-color: #3d5485;
    color: #fff;
}

.add-btn:hover,
.add-btn:active,
.update-btn:hover,
.update-btn:active,
.add-btn:focus,
.update-btn:focus {
    background-color: #2b3b5e;
    border-color: #2b3b5e;
    color: #fff;
}

.add-btn:focus,
.update-btn:focus {
    box-shadow: 0 0 0 0.25rem rgb(61 84 133 / 25%);
}

.add-btn.affectingEast,
.update-btn.affectingEast {
    background-color: #e5b4de;
    border-color: #e5b4de;
    color: #000;
}

.add-btn.affectingEast:focus,
.update-btn.affectingEast:focus {
    box-shadow: 0 0 0 0.25rem rgb(229 180 222 / 25%);
}
</style>