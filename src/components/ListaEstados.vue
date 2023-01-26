<template >
    <div class="Container">
        <h1>Codigos Estados</h1>
        <ul id="lista-estados">
            <li v-for="item in listaEstados">
                {{ item.state }}
            </li>
        </ul>
    </div>
    <input v-on:keypress.enter="buscarEstado()" type="text" v-model="estadoId">
    <div class="container-resultado" v-if="estado">
        <h1>state </h1>
        <input type="text" v-model="estado">
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
            const { state, positive, totalTestResults, hospitalizedCurrently, death, totalTestsViral, deathIncrease } = await this.APIlistEstadosPorEstate(this.estadoId)
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
        console.log('mounted')
    },
    beforeCreate() {
        console.log('beforeCreate')
    },
    created() {
        console.log('created')
    },
    beforeMount() {
        console.log('beforeMount')
    },
    updated() {
        console.log('updated')
    },
    activated() {
        console.log('activated')
    },
    beforeUnmount() {
        console.log('beforeUnmount')
    },
    unmounted() {
        console.log('unmounted')
    },
    renderTracked() {
        console.log('renderTracked')
    },
    errorCaptured() {
        console.log('errorCaptured')
    },
    deactivated() {
        console.log('deactivated')
    },

}
</script>
<style >
#lista-estados {
    display: grid;
    grid-template-columns: auto auto auto;
    list-style: none;
}

.container-resultado {
    background-color: gray;
    width: 300px;
    text-align: center;
    margin: 10px auto;
}

.container-resultado h1 {
    font-size: 90%;
}
</style>