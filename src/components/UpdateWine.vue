<script>
import axios from 'axios'
import WineTypes from './WineTypes.vue';
import Countries from './Countries.vue';

export default {
    name: "UpdateWine",
    components: {
        WineTypes,
        Countries
    },
    props: {
        selected: Object,
    },
    data() {
        return {
            tempScore: this.selected.score
        };
    },
    methods: {
    updateWine() {
        this.selected.score = this.tempScore;
        axios.put("/wine/"+ this.selected.id, this.selected)
        .then(
            this.update_dialog = false,
            location.reload()
        )
        .catch((error) => {
            window.alert(`The API returned an error: ${error}`)
        })
    },
    onCountryAdded() {
        this.$emit('country-added');
        },
    onTypeAdded() {
        this.$emit('type-added');
        }
    }
}
</script>

<template>
    <Dialog :style="{ width: '25rem' }" header="Wine Details" modal>
        <div class="flex items-center gap-4 mb-4">
            <label for="name" class="font-semibold w-24">Name</label>
            <InputText id="name" v-model="selected.name" class="w-full md:w-[14rem]" autocomplete="off"/>
        </div>
        <WineTypes :selected="selected" @type-added="onTypeAdded" />
        <Countries :selected="selected" @country-added="onCountryAdded"/>
        <div class="flex items-center gap-4 mb-4">
                <label for="score" class="font-semibold w-24">Score</label>
                <Rating v-model="tempScore" />
        </div>
        <template #footer>
            <Button label="Cancel" icon="pi pi-times" text @click="this.$parent.wine_dialog = false" />
            <Button label="Save" icon="pi pi-check" @click="updateWine" />
        </template>
    </Dialog>
</template>