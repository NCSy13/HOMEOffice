<template><v-btn color="blue-darken-1" variant="text" @click="testRouter">return</v-btn>
</template>
<script>
import axios from 'axios'
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
  },
  methods: {
    testRouter(){
      this.$router.push('/')
    }
  }
}
</script>