<template>
  <div id="app">
    <img alt="App logo" src="./assets/carrito.png">
    <h1 class="text-center">{{ title }}</h1>
    <br>
    <div class="container">
        <section class="form">
            <form action="">
                <div class="form-row">
                    <!-- Formulario producto-->
                    <div class="col">
                        <v-select label="name" :options="shops" v-model="selected.shop"></v-select>
                    </div>
                    <div class="col">
                        <v-select label="name" :options="products" v-model="selected.product"></v-select>
                    </div>
                    <div class="col">
                        <input v-model="selected.q" placeholder="Cantidad" class="form-control">
                    </div>
                    <div class="col">
                        <!-- Botón para añadir -->
                        <input v-on:click="add" type="button" value="Add Order" class="btn btn-success">
                    </div>
                </div>
            </form>
        </section>
        <br>
        <!-- Tabla donde se muestran los datos -->
        <section class="data">
            <table class="table table-striped table-hover">
                <thead class="thead-dark">
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Comercio</th>
                    <th scope="col">Producto</th>
                    <th scope="col">Cantidad</th>
                    <th scope="col">Precio Unitario</th>
                    <th scope="col">Precio Total</th>
                    <th></th>
                </tr>
                </thead>
                <tbody>
                <tr v-for="(order, index) in orders" v-bind:key="order.id">
                    <td>{{ order.id }}</td>
                    <td>
                      <span v-if="edit_mode && edited.id === index">
                        <input v-model="edited.shop" class="form-control">
                      </span>
                      <span v-else>{{ order.shop }}</span>
                    </td>
                    <td>
                      <span v-if="edit_mode && edited.id === index">
                        <input v-model="edited.product" class="form-control">
                      </span>
                      <span v-else>{{ order.product }}</span>
                    </td>
                    <td>
                    <span v-if="edit_mode && edited.id === index">
                            <input v-model="edited.q" class="form-control">
                          </span>
                          <span v-else>{{ order.q }}</span>
                    </td>
                    <td>
                      <span v-if="edit_mode && edited.id === index">
                        <input v-model="edited.amount" class="form-control">
                      </span>
                        <span v-else>{{ order.amount }}</span>
                    </td>
                    <td>
                      <span v-if="edit_mode && edited.id === index">
                        <input v-model="edited.totalAmount" class="form-control">
                      </span>
                      <span v-else>{{ order.totalAmount }}</span>
                    </td>
                    <td>
                      <span v-if="edit_mode && edited.id === index">
                        <button v-on:click="update(index)" class="btn btn-secondary">Guardar</button>
                      </span>
                      <span v-else>
                        <button v-on:click="edit(index)" class="btn btn-info">Actualizar</button>
                        <button v-on:click="remove(index)" class="btn btn-danger">Borrar</button>
                      </span>
                    </td>
                </tr>
                </tbody>
            </table>
        </section>
      </div>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        title: 'Mis pedidos',
        selected: {},
        edit_mode: false,
        edited: {},
        shops: [
            {
                name: 'La Farola de Urquiza',
                location: 'Av. Monroe 5002, C1431 CAP, Buenos Aires'
            },
            {
                name: 'El Imperio de la Pizza',
                location: '1427, Av. Corrientes 6891, C1427 BPG, Buenos Aires'
            }
        ],
        products: [
            {name: 'Pizza Fugazzeta',amount:230.00},
            {name: 'Pizza Muzzarella',amount:300.00},
            {name: 'Cerveza',amount:180.00},
            {name: 'Bebida',amount:80.00},
            {name: 'Milanesa de Pollo Napolitana',amount:230.00},
            {name: 'Pizzanesa',amount:300.00},
        ],
        orders: [
          {
            id: Math.floor(Math.random() * 100000),
            shop: 'La Farola de Urquiza',
            product: 'Milanesa de Pollo Napolitana',
            q: 2,
            amount: 230.00,
            totalAmount: 460.00
          },
          {
            id: Math.floor(Math.random() * 100000),
            shop: 'La Farola de Urquiza',
            product: 'Bebida',
            q: 2,
            amount: 80.00,
            totalAmount: 160.00
          },
          {
            id: Math.floor(Math.random() * 100000),
            shop: 'El Imperio de la Pizza',
            product: 'Pizza Fugazzeta',
            amount: 300.00,
            q: 1,
            totalAmount: 300.00
          },
          {
            id: Math.floor(Math.random() * 100000),
            shop: 'El Imperio de la Pizza',
            product: 'Cerveza',
            amount: 180.00,
            q: 1,
            totalAmount: 180.00
          }
        ]
      }
    },
    methods: {
      add: function(){
        this.orders.push({
            id: Math.floor(Math.random() * 100000),
            shop: this.selected.shop.name,
            product: this.selected.product.name,
            q: this.selected.q,
            amount: this.selected.product.amount,
            totalAmount: this.selected.q * this.selected.product.amount
        });
        this.selected = {};
      },
      remove: function(index){
        this.orders.splice(index, 1);
      },
      edit: function(index){
        this.edit_mode = true;

        this.edited.id = index;
        this.edited.shop = this.orders[index].shop;
        this.edited.product = this.orders[index].product;
        this.edited.q = this.orders[index].q;
        this.edited.amount = this.orders[index].amount;
        this.edited.totalAmount = this.orders[index].totalAmount;
      },
      update: function(index){
        this.edit_mode = false;

        this.orders[index].shop = this.edited.shop;
        this.orders[index].product = this.edited.product;
        this.orders[index].q = this.edited.q;
        this.orders[index].amount = this.edited.amount;
        this.orders[index].totalAmount = this.edited.q * this.edited.amount;
      }
    }
  }
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
