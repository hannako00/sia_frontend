<template>
    <div>
        <button class="btn btn-primary float-right" @click="onNew">
            New Car
        </button>
        <h5>Car View</h5>
        <hr>

        <form action="" @submit.prevent="onSave">
            <b-form-group label="Brand">
                <b-form-input v-model="car.brand"></b-form-input>
            </b-form-group>
            <b-form-group label="Model">
                <b-form-input v-model="car.model"></b-form-input>
            </b-form-group>
            <b-form-group label="Plate Number">
                <b-form-input v-model="car.plate_number"></b-form-input>
            </b-form-group>
            <b-form-group label="Color">
                <b-form-input v-model="car.color"></b-form-input>
            </b-form-group>
            <b-form-group label="Value">
                <b-form-input v-model="car.value"></b-form-input>
            </b-form-group>

            <b-form-group>
                <button class="btn btn-success" type="submit">Save Changes</button>
                <button class="btn btn-danger float-right" type="button" @click="onDelete" v-if="car.id">Delete Car</button>
            </b-form-group>
        </form>
    </div>
</template>

<script>
export default {
    props: {
        car: {}
    },
    methods: {
        async onSave() {
            try {
                if(!this.car.id) {
                    await this.$axios.post('/api/cars', this.car)
                }else{
                    await this.$axios.put('/api/cars/' + this.car.id, this.car)
                }

                this.$emit('saved')
            } catch (err) {
                alert(err.response.data.message)
            }
        },
        onNew() {
            this.$emit('newCar')
        },
        async onDelete() {
            try {
                this.$axios.delete('/api/cars/' + this.car.id)
                this.$emit('deleted')
            } catch (err) {
                alert(err.response.data.message)
            }
        }
    }
}
</script>
