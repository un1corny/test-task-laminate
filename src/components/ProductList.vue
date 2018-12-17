<template>
    <main class="grid container">
        <product v-for="(item, index) in items"
                 :key="index"
                 v-bind:index="index"
                 v-bind:item="item">

        </product>
    </main>
</template>

<script>
    import Product from './Product.vue'

    export default {
        name: 'ProductList',
        components: {
            "product": Product
        },
        props: [],
        data() {
            return {
                items: []
            };
        },
        mounted() {
            this.axios
                .get('/products.json')
                .then(response => {
                    this.items = response.data.map(item => {
                        item.assocProducts = item.assocProducts.split(' ');
                        return item;
                    });
                })
                .catch(error => alert(error));

        },

    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">

</style>
