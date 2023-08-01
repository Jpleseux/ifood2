<template>
      <div id="burger-table" >
        <Message :msg="msg" v-show="msg"/>
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
    </div>
    <div id="burger-table-rows">
      <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{burger.nome}}</div>
        <div>{{ burger.pao }}</div>
        <div>{{ burger.carne }}</div>
        <div>
          <ul>
            <li v-for="(opcional, index) in burger.opcionais">
            {{ opcional }}
        </li>
          </ul>
        </div>
        <div>
          <select name="status" class="status" @change="updateBurger($event.target.value, burger.id)">
            <option >Status burger
            </option>
            <option v-for="statu in  status" :key="statu.id" :value="statu.tipo" :selected="burger.status ==statu.tipo">{{ 
            statu.tipo }}</option>
          </select>
          <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from './Message.vue'

export default{
    name: "Dashboard",
    data() {
        return {
            burgers: null,
            burgers_id: null,
            status: [],
            msg: null
        };
    },
    methods: {
        async getPedidos() {
            const req = await fetch("http://localhost:3000/burgers");
            const data = await req.json();
            this.burgers = data;
            this.getStatus();
        },
        async getStatus() {
            const req = await fetch("http://localhost:3000/status");
            const data = await req.json();
            this.status = data;
            console.log(this.status);
        },
        async deleteBurger(id) {
            const req = await fetch("http://localhost:3000/burgers/" + id, {
                method: "DELETE"
            });
            const res = await req.json();
            //msg
            this.msg = `Pedido Nª ${id} foi deletado com sucesso`

            setTimeout(()=>{
                this.msg= null
            }, 4000)

            this.getPedidos();
        },
        async updateBurger(option, id) {
            const dataj = JSON.stringify({ status: option });
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH",
                headers: {
                    "Content-Type": "application/json"
                },
                body: dataj
            });
            const res = await req.json();

            this.msg = `Pedido Nª ${res.id} foi atualizado com sucesso`

            setTimeout(()=>{
                this.msg= null
            }, 4000)
            console.log(res);
        }
    },
    mounted() {
        this.getPedidos();
    },
    components: { Message }
}
</script>

<style scoped>

#burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }

  select {
    margin-bottom: 2%;
    width: 100%;
    padding: 4% 6%;
    margin-right: 2%;
  }

  .delete-btn {
    background-color: #222;
    color:#fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
  }
  
  .delete-btn:hover {
    background-color: transparent;
    color: #222;
  }
</style>