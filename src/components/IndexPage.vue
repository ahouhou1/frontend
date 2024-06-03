<template>
    <div class="IndexPage">
        <br>
        <div class="row">
            <div class="col-md-9">
                <div class="row gx-4 gy-4 row-cols-3">
                    <div class="col" v-for="product in list_products" :key="product.id">
                        <div class="p-3 border bg-light">
                            <h5>{{ product.name }}</h5>
                            <p>{{ product.desc }}</p>
                            <p>{{ product.price }}€</p>
                            <button type="button" class="btn btn-success btn-sm rounded-button" @click="add_to_cart(product.id)">Ajouter au panier</button>
                        </div>
                    </div>
                </div>
            </div>

            <div class="col-6 col-md-3">
                <div class="col">
                    <div class="p-3 border bg-success text-white">
                        <h5>Cart</h5>
                    </div>
                </div>
                <div class="row gx-4 row-cols-1">
                    <div class="col" v-for="(value, id) in list_cart" :key="id">
                        <div class="p-3 border bg-light">
                            <h5>{{ get_info_product(id).name || 'Product Not Found' }}</h5>
                            <h6>Quantité: {{ value }}</h6>
                            <button type="button" class="btn btn-danger btn-sm rounded-button" @click="remove_from_cart(id)">Supprimer</button>
                        </div>
                    </div>

                    <div class="col">
                        <div class="p-3 border bg-success text-white">
                            <h5>Total: {{ total_price }}€</h5>
                            <h6>Nombre de produits: {{ nbr_product }}</h6>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';

export default {
    name: 'IndexPage',
    data() {
        return {
            list_products: [],
            list_cart: {},
            total_price: 0,
            nbr_product: 0
        };
    },
    async created() {
        await this.fetchProducts();
    },
    methods: {
        async fetchProducts() {
            try {
                const response = await axios.get('http://localhost:3000/products');
                this.list_products = response.data;
                console.log('Products fetched:', this.list_products); // Debug log
            } catch (error) {
                console.error("Failed to fetch products:", error);
            }
        },
        add_to_cart(id) {
            console.log('Adding to cart:', id);
            if (this.list_cart[id]) {
                this.list_cart[id]++;
            } else {
                this.list_cart[id] = 1;
            }
            this.update_total();
        },
        get_info_product(id) {
            const productId = parseInt(id);
            const product = this.list_products.find(product => product.id === productId);
            console.log('Get info product:', id, product);
            return product || {};
        },
        update_total() {
            this.total_price = Object.entries(this.list_cart).reduce((total, [id, qty]) => {
                const product = this.get_info_product(id);
                return total + (product.price * qty);
            }, 0);
            this.nbr_product = Object.values(this.list_cart).reduce((total, qty) => total + qty, 0);
        },
        remove_from_cart(id) {
            console.log('Removing from cart:', id);
            if (this.list_cart[id] > 1) {
                this.list_cart[id]--;
            } else {
                delete this.list_cart[id];
            }
            this.update_total();
        }
    }
};
</script>

<style scoped>
.IndexPage {
    padding: 20px;
}

.row {
    display: flex;
    flex-wrap: wrap;
}

.col-md-9 {
    flex: 0 0 75%;
    max-width: 75%;
}

.col-6.col-md-3 {
    flex: 0 0 25%;
    max-width: 25%;
}

.border {
    border: 1px solid #dee2e6;
    border-radius: 4px;
}

.bg-light {
    background-color: #f8f9fa;
}

.bg-success {
    background-color: #28a745;
}

.text-white {
    color: white;
}

.btn-success {
    background-color: #28a745;
    border-color: #28a745;
}

.btn-danger {
    background-color: #dc3545;
    border-color: #dc3545;
}

.p-3 {
    padding: 1rem;
}

.h5, .h6 {
    margin: 0;
}

.rounded-button {
    border-radius: 50px;
    box-shadow: none;
}
</style>
