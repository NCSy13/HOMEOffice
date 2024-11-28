<template>
    <v-data-table :headers="headers" :items="desserts" :items-per-page="10" class="elevation-1">
        <template v-slot:top>
            <v-toolbar flat>
                <v-toolbar-title>My CRUD</v-toolbar-title>
                <v-divider class="mx-4" inset vertical></v-divider>
                <v-spacer></v-spacer>
                <v-dialog v-model="dialog" max-width="500px">
                    <template v-slot:activator="{ props }">
                        <v-btn class="mb-2" color="primary" dark v-bind="props">
                            New Item
                        </v-btn>
                    </template>
                    <v-card>
                        <v-card-title>
                            <span class="text-h5">{{ formTitle }}</span>
                        </v-card-title>

                        <v-card-text>
                            <v-container>
                                <v-row>
                                    <v-col cols="12" md="4" sm="6">
                                        <v-text-field v-model="editedItem.name" label="Dessert name"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" md="4" sm="6">
                                        <v-text-field v-model="editedItem.calories" label="Calories"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" md="4" sm="6">
                                        <v-text-field v-model="editedItem.fat" label="Fat (g)"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" md="4" sm="6">
                                        <v-text-field v-model="editedItem.carbs" label="Carbs (g)"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" md="4" sm="6">
                                        <v-text-field v-model="editedItem.protein" label="Protein (g)"></v-text-field>
                                    </v-col>
                                    <v-col cols="12" md="4" sm="6">
                                        <v-text-field v-model="editedItem.iron" label="Protein (g)"></v-text-field>
                                    </v-col>
                                </v-row>
                            </v-container>
                        </v-card-text>

                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue-darken-1" variant="text" @click="close">
                                Cancel
                            </v-btn>
                            <v-btn color="blue-darken-1" variant="text" @click="save">
                                Save
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
                <v-dialog v-model="dialogDelete" max-width="500px">
                    <v-card>
                        <v-card-title class="text-h5">Are you sure you want to delete this item?</v-card-title>
                        <v-card-actions>
                            <v-spacer></v-spacer>
                            <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancel</v-btn>
                            <v-btn color="blue-darken-1" variant="text" @click="deleteItemConfirm">OK</v-btn>
                            <v-spacer></v-spacer>
                        </v-card-actions>
                    </v-card>
                </v-dialog>
            </v-toolbar>
        </template>
        <template v-slot:item.actions="{ item }">
            <v-icon class="me-2" size="small" @click="editItem(item)">
                mdi-pencil
            </v-icon>
            <v-icon size="small" @click="deleteItem(item)">
                mdi-delete
            </v-icon>
            <v-btn color="blue-darken-1" variant="text" @click="testRouter">testRouter</v-btn>

        </template>
    </v-data-table>
</template>
<script>
import axios from 'axios'
import { get, post, put, deleteData } from '../http/index'
export default {
    data() {
        return {
            dialog: false,
            dialogDelete: false,
            headers: [
                {
                    title: 'Dessert (100g serving)',
                    align: 'start',
                    sortable: false,
                    value: 'name',
                },
                { title: 'Calories', value: 'calories' },
                { title: 'Fat (g)', value: 'fat' },
                { title: 'Carbs (g)', value: 'carbs' },
                { title: 'Protein (g)', value: 'protein' },
                { title: 'Iron (%)', value: 'iron' },
                { title: 'Actions', key: 'actions', sortable: false },
            ],
            desserts: [], editedItem: {
                name: '',
                calories: '',
                fat: '',
                carbs: '',
                protein: '',
                iron: '',
                id: null,
            },
            defaultItem: {
                name: '',
                calories: '',
                fat: '',
                carbs: '',
                protein: '',
                iron: '',
                id: null,
            }
        }
    }, mounted() {
        this.getData();
    },
    methods: {
        async getData() {
            // axios.get("http://localhost:3000/test").then(
            //     response => {
            //         console.log(response.data);
            //         this.desserts = response.data;
            //     },
            //     error => {
            //         console.log(error);
            //     }
            // );
            this.desserts = await get()
        },
        editItem(item) {
            this.editedItem = Object.assign({}, item)
            this.dialog = true
        },
        deleteItem(item) {
            this.editedItem = Object.assign({}, item)
            this.dialogDelete = true
        },
        closeDelete() {
            this.dialogDelete = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
            })
        },
        close() {
            this.dialog = false
            this.$nextTick(() => {
                this.editedItem = Object.assign({}, this.defaultItem)
            })
        },
        async save() {
            // console.log(this.editedItem)
            if (this.editedItem.id == null) {
                this.editedItem.id = this.desserts[this.desserts.length - 1].id + 1
                // axios.post('http://localhost:3000/test', this.editedItem)
                //     .then(response => {
                //         this.getData();
                //     })
                //     .catch(error => {
                //         console.error(error);
                //     });
                await post(this.editedItem)
            } else {
                // axios.put('http://localhost:3000/test/' + this.editedItem.id, this.editedItem)
                //     .then(response => {
                //         this.getData();
                //     })
                //     .catch(error => {
                //         console.error(error);
                //     });
                await put(this.editedItem.id, this.editedItem)
            }
            this.getData();
            this.close()
        },
        async deleteItemConfirm() {
            // axios.delete('http://localhost:3000/test/' + this.editedItem.id)
            //     .then(response => {
            //         this.getData();
            //     })
            //     .catch(error => {
            //         console.error(error);
            //     });

            await deleteData(this.editedItem.id)
            this.getData()
            this.closeDelete()
        },
        testRouter() {
            this.$router.push("/testPage")
        },
    }
}
</script>