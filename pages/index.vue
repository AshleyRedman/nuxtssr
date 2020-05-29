<template>
  <main>
    <div>
      <!-- V Model links this to the varialble given to it, in this case 'query' -->
      <!-- the '@keyup=' is an event, as in when someone types in, run this thing which points
      to the 'searchProducts method we have below -->
      <input v-model="query" type="text" @keyup="searchProducts" />
    </div>
    <div>
      <ul v-if="products">
        <li v-for="product in products" :key="product.id">
          <a :href="product.link">{{ product.title.rendered }}</a>
        </li>
      </ul>
    </div>
  </main>
</template>

<script>
export default {
  components: {},
  data() {
    // Data is just a list of variables we will use in this component, usually overritten at some point
    return {
      products: '', // a placeholder for the products we want
      query: '' // a placeholder for what the user types into the input box
    };
  },
  created() {
    // created is run when this file (component) is rendered, i.e. loaded on the page
    this.searchProducts();
  },
  methods: {
    // methods is more or less a collection of functions you want
    async searchProducts() {
      try {
        const res = await this.$axios({
          method: 'get',
          url: '/product',
          params: {
            per_page: 100,
            search: this.query,
            status: 'publish',
            order: 'asc'
          }
        });

        // When we get back a nice JSON list of products, set it to our variable above called products to loop over and show
        this.products = res.data;
      } catch (err) {
        // if the above request fails for whatever reason, for now just log it in the console
        console.log(err);
      }
    }
  }
};
</script>
