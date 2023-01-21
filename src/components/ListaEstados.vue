<template >
    <div class="Container">
        <h1>Codigos Estados</h1>
        <ul id="lista-estados">
            <li v-for="item in listaEstados">
                {{ item.state }}
            </li>
        </ul>
    </div>

    <button v-on:click="iniciar"> Cargar lista </button>
    <input type="text" v-model="estadoId">
    <button v-on:click="buscarEstado">Buscar estado</button>
    <div v-if="resultadp">
        <h1>state </h1>
        <input type="text" v-model="Estado">
        <h1>positive</h1>
        <input type="text" v-model="positivos">
        <h1>totalTestResults</h1>
        <input type="text" v-model="totalResul">
        <h1>hospitalizedCurrently</h1>
        <input type="text" v-model="hospitalizados">
        <h1>death</h1>
        <input type="text" v-model="muertos">
        <h1>totalTestsViral</h1>
        <input type="text" v-model="totalViral">
        <h1>deathIncrease</h1>
        <input type="text" v-model="mueresIncremento">
    </div>
</template>
<script>
export default {
    data() {
        return {
            listaEstados: null,
            estadoId: null,
            resultadp: false,

            Estado: null,
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

        async contruirLista() {
            const vectorRetornoEstados = []
            const vectorObjetosEstados = this.APIlistEstados()
            for (let i = 0; i < 57; i++) {
                const obj = await this.contruirObjetoLista(state)
                vectorRetornoEstados.unshift(obj)
            }
            return vectorRetornoEstados


        },
        async contruirObjetoLista(estado) {
            const { state, positive, totalTestResults, hospitalizedCurrently, death, totalTestsViral, deathIncrease } = this.APIlistEstadosPorEstate(estado)
            const objetoEstado = { Estado: state, positivos: positive, totalResul: totalTestResults, hospitalizados: hospitalizedCurrently, muertos: death, totalViral: totalTestsViral, mueresIncremento: deathIncrease }
            return objetoEstado
        },
        async buscarEstado() {
            this.resultadp = true
            const { state, positive, totalTestResults, hospitalizedCurrently, death, totalTestsViral, deathIncrease } = await this.contruirObjetoLista(this.estadoId)
            this.Estado = state
            this.positivos = positive

        }
    }
}
</script>
<style >
#lista-estados {
    display: grid;
    grid-template-columns: auto auto auto;
    list-style: none;
}
</style>