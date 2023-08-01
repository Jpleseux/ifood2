<template>
    <Message :msg="msg" v-show="msg"/>
    <div>
        <form id="burguer" @submit="createBurger">
            <div class="input-container">
                <label for="name">
                    Client name:
                </label>
                <input 
                type="text" id="name" 
                v-model="nome"
                placeholder="type your name"
                />
            </div>
            <div class="input-container">
                <label for="pão">
                    choose the bread:
                </label>
                <select  name="pao" id="pao" v-model="pao">
                    <option value="">Selecione seu pão</option>
                <option
                v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{ pao.tipo }}</option>
                </select>
            </div>
            <div class="input-container">
                <label for="pão">
                    choose meat of your burger:
                </label>
                <select name="carne" id="carne" v-model="carne">
                    <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{ carne.tipo }}</option>
            <optio value=""></optio>
                </select>
            </div>
            <div   
            id="opcionais-container"
            class="input-container">
                <label id="optional-tittle" for="pão">
                    choose the optional:
                </label>
                <div class="checkbox-container" v-for="optional in opcionaisData" :key="optional.id">
                    <input type="checkbox" name="optional" v-model="opcionais" :value="optional.tipo">
                    <span>{{ optional.tipo }}</span>
                </div>
            </div>
            <div class="input-container">
                <input type="submit"
                    class="submit-btn"
                    value="Criar seu burguer"
                >
            </div>
        </form>
    </div>
</template>

<script>
import Message from './Message.vue'

export default{
    name: "BurgerForm",
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisData: null,
            nome: null,
            pao: null,
            carne: null,
            opcionais: [],
            status: "Solicitado",
            msg: null
        };
    },
    methods: {
        async getIngredientes() {
            const req = await fetch("http://localhost:3000/ingredientes", {
                method: "GET",
                headers: {
                    "Content-Type": "application/json",
                }
            });
            const data = await req.json();
            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisData = data.opcionais;
        },
        async createBurger(e) {
            e.preventDefault();
            const data = {
                nome: this.nome,
                carne: this.carne,
                pao: this.pao,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado "
            };
            const datajson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: datajson
            });
            const res = await req.json();

            this.msg = `Pedido nº ${res.id} realizado com suceso`

            setTimeout(() => {
                this.msg = null
            }, 3000);

            this.nome = "";
            this.carne = "";
            this.pao = "";
            this.opcionais = "";
        }
    },
    mounted() {
        this.getIngredientes();
    },
    components: { Message 
    }
}
</script>
<style scoped>
#burger-form{
    max-width: 400px;
    margin: 0 auto;
}
.input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 5%;
}
label{
    font-weight: bold;
    margin-bottom: 2%;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #fcba03;
}

input, select{
    padding:5px 10px ;
    width:60%;
}

#opcionais-container{
    flex-direction: row;
    flex-wrap: wrap;
}
#optional-tittle{
    width: 100%;
}
.checkbox-container{
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}
.checkbox-container span, .checkbox-container input{
    width: auto;
}
.checkbox-container span{
    margin-left: 2%;
    font-weight: bold;
}
.submit-btn{
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    cursor: pointer;
    font-size: 16px;
    margin: 0 auto;
    transition: .5s;
}
.submit-btn:hover{
    background-color: transparent;
    color: #222;
}
</style>