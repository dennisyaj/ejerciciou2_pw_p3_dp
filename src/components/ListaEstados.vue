<template >
    <div class="container-lista">
        <h1>Códigos de Estados</h1>
        <ul id="lista-estados">
            <li v-for="item in listaEstados">
                {{ item.state }}
            </li>
        </ul>
    </div>
    <input v-on:keypress.enter="buscarEstado()" type="text" v-model="estadoId" placeholder="Ingrese código de estado">
    <div class="container-resultado" v-if="estado">
        <h2>Casos positivos:</h2>
        <input type="text" v-model="positivos" readonly>
        <h2>Total de resultados de pruebas:</h2>
        <input type="text" v-model="totalResul" readonly>
        <h2>Actualmente hospitalizados:</h2>
        <input type="text" v-model="hospitalizados" readonly>
        <h2>Muertos</h2>
        <input type="text" v-model="muertos" readonly>
        <h2>Casos positivos virales:</h2>
        <input type="text" v-model="totalViral" readonly>
        <h2>Número de aumento de muertes:</h2>
        <input type="text" v-model="mueresIncremento" readonly>
    </div>
</template>
<script>
export default {
    data() {
        return {
            listaEstados: null,
            estadoId: null,
            estado: null,
            positivos: null,
            totalResul: null,
            hospitalizados: null,
            muertos: null,
            totalViral: null,
            mueresIncremento: null
        }
    },
    methods: {
        async iniciar() {
            this.listaEstados = await this.APIlistEstados()
        },
        async APIlistEstados() {
            const data = await fetch("https://api.covidtracking.com/v1/states/current.json").then((r) => r.json())
            return data
        },
        async APIlistEstadosPorEstate(state) {
            const data = await fetch("https://api.covidtracking.com/v1/states/" + state + "/current.json").then((r) => r.json())
            return data
        },
        async buscarEstado() {
            const { state, positive, totalTestResults, hospitalizedCurrently, death, totalTestsViral, deathIncrease } = await this.APIlistEstadosPorEstate(this.estadoId.toLowerCase())
            this.estado = state
            this.positivos = positive
            this.totalResul = totalTestResults
            this.hospitalizados = hospitalizedCurrently
            this.muertos = death
            this.totalViral = totalTestsViral
            this.mueresIncremento = deathIncrease
        }
    },
    mounted() {
        this.iniciar()
    },
}
</script>
<style >
#lista-estados {
    display: grid;
    grid-template-columns: auto auto auto auto;
    list-style: none;
}

.container-resultado {
    background-color: rgb(179, 175, 175);
    width: 300px;
    margin: 0px auto;
    text-align: left;
    margin-top: 30px;
    padding-bottom: 15px;
    border-radius: 5px;
}

.container-resultado h2,
input {
    margin-left: 15px;
    text-align: left;
    font-size: 90%;
}


.container-resultado h2 {
    padding-top: 15px;
}

input {
    border-radius: 5px;
    padding-left: 5px;
    border-style: solid;

}

.container-lista {
    width: 35vw;
    margin: 0px auto;
}
</style>