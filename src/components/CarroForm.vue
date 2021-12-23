<template>  
    <div>
        <Mensagem :msg="msg" v-show="msg" />

        <form @submit="createCarro" id="carro-form">
            
            <div class="input-container">
                <label for="nome">Nome da Pessoa: </label>
                <input type="text" v-model="nome" id="nome" placeholder="Informe o seu nome: ">
            </div>

            <div class="input-container">
                <label for="nomeCarro">Modelo do Carro: </label>
                <input type="text" v-model="nomeCarro" id="nomeCarro" placeholder="Informe o modelo do Carro: ">
            </div>

            <div class="input-container">
                <label for="placaCarro">Placa do Carro: </label>
                <input type="text" v-model="placaCarro" id="placaCarro" placeholder="Informe a placa do carro: ">
            </div>

            <div class="input-container">
                <label for="hora">Horário de entrada: </label>
                <input type="text" v-model="hora" id="hora" placeholder="Informe o horário de entrada: ">
            </div>

            <div class="input-container">
                <input type="submit" class="submit-btn" value="Cadastrar Carro">
            </div>

        </form>
    </div>
</template>

<script>
    import Mensagem from "./Mensagem.vue"

    export default {
        name: "CarroForm",
        data(){
            return {

                nome: null,
                nomeCarro: null,
                placaCarro: null,
                hora: null,
                msg: null
            }
        },
        
        methods: {
            async createCarro(e){
                e.preventDefault()

                //console.log("Carro cadastrado com sucesso!")

                const data = {
                    nome: this.nome,
                    nomeCarro: this.nomeCarro,
                    placaCarro: this.placaCarro,
                    hora: this.hora,
                    status: "Solicitado"
                }

                //console.log(data)

                const dataJson = JSON.stringify(data)
                
                // Fazendo o POST
                const req = await fetch("http://localhost:3000/carros", {
                    method: "POST",
                    headers: { "Content-Type" : "application/json"},
                    body: dataJson
                })

                const res = await req.json()

                //console.log(res)

                // Mensagem de cadastro de carro
                this.msg = `Carro modelo ${data.nomeCarro}, do(a) cliente ${data.nome} cadastrado com sucesso`

                // Limpar a Mensagem após um tempo
                setTimeout(() => this.msg = "", 6000)

                // Limpar campos
                this.nome = ""
                this.nomeCarro = ""
                this.placaCarro = ""
                this.hora = ""
            }
        },

        components: {
            Mensagem
        }
    }
</script>

<style scoped>
    #carro-form{
        max-width:300px;
        margin: 0 auto;
    }

    .input-container {
        display: flex;
        flex-direction: column;
        margin-bottom: 20px;
    }

    label {
        font-weight: bold;
        margin-bottom: 15px;
        color: #222;
        padding: 5px 10px;
        border-left: 4px solid #3F7FBF;
    }

    input {
        padding: 5px 10px;
        widows: 300px;
    }

    .submit-btn {
        background-color: #3F7FBF;
        color: white;
        font-weight: bold;
        padding: 10px;
        font-size: 16px;
        border: 2px solid white;
        cursor: pointer;
        transition: .5s;
    }

    .submit-btn:hover {
        background-color: tomato;
        font-size: 18px;
    }
</style>