<template>
    <div>
        <NavBar />
        <div class="container">
            <h1>My Cars</h1>
            <div class="row">
                <div class="col-6">
                    <h5>List of Cars</h5>
                    <hr>
                    <ul class="list-group">
                        <li class="list-group-item btn-li"  v-for="car in cars" :key="car.id" @click="onSelected(car)">
                            {{car.brand}} <span class="float-right">{{car.value}}</span>
                        </li>
                    </ul>
                </div>
                <div class="col-4">
                    <CarView :car="selectedCar" @saved="onChange" @newCar="onNew" @deleted="onChange" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            cars: [],
            selectedCar: {}
        }
    },
    methods: {
        async getMyCars() {
            await this.$axios.get('/api/cars')
            .then((res)=>{
                if(res.status==200) {
                    this.cars = res.data
                }
            })
        },
        onChange() {
            this.getMyCars()
            this.selectedCar = {}
        },
        onSelected(car) {
            this.selectedCar = car;
        },
        onNew() {
            this.selectedCar = {}
        },
    },
    created() {
        this.getMyCars()
    }
}
</script>

<style>
.row {
    padding-top: 30px;
}
.container {
    margin-top: 20px;
}
h1 {
    text-align: center;
}
.btn-li {
    cursor: pointer;
}
.btn-li:hover {
    background-color: antiquewhite;
}
</style>
