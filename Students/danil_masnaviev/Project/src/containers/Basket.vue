<template>
    <div class="cart-block ">
        <div class="d-flex">
            <strong class="d-block">Всего товаров</strong>
            <div id="quantity"></div>
        </div>
        <hr>
        <div class="cart-items">
            <item v-for="item of items" :key="item.id_product" :type="'basket'" :item="item"/>
        </div>
        <hr>
        <div class="d-flex">
            <strong class="d-block">Общая ст-ть:</strong>
            <div id="price"></div>
        </div>
    </div>
</template>

<script>
    import item from '../components/Item.vue'
    export default {
        components: { item },
        data() {
            return {
                items: [],
                url: 'https://raw.githubusercontent.com/GeekBrainsTutorial/online-store-api/master/responses/getBasket.json',
            }
        },
        mounted() {
            this.$parent.get(this.url).then(d => {
                this.items = d.contents;
            })
        },
        methods: {
            add(item) {
                let find = this.items.find(el => el.id_product == item.id_product);
                if (find == undefined) {
                    let newItem = {
                        id_product: item.id_product,
                        img: '',
                        quantity: 1,
                        product_name: item.product_name,
                        price: item.price
                    };
                    this.items.push(newItem);
                } else {
                    find.quantity++;
                }
                console.log('added ' + item.product_name)
            },
            remove(item) {
                let find = this.items.find(el => el.id_product == item.id_product);
                if (find.quantity < 2){
                    let pos = this.items.indexOf(find);
                    this.items.splice(pos, 1);
                }
                else
                {
                    find.quantity--;
                }
                console.log('removed ' + item.product_name)
            }
        }
    }
</script>

<style>
</style>