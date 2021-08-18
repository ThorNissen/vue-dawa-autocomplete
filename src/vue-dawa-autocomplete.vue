<template>
    <div class="position-relative">
        <input
            :id="id"
            type="text"
            v-model="address"
            @keyup="autocomplete"
            @click="autocomplete"
            :disabled="disabled"
            autocomplete="new-text"
        />

        <div id="suggestion-wrapper">
            <div
                class="suggestion"
                v-for="(suggestion, s) in suggestions"
                :key="`suggestion_${s}`"
                @click="selectAddress(suggestion)"
                :class="{ 'active-item': currentItem === s }"
            >
                {{ suggestionText(suggestion) }}
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import _ from 'lodash';

export default {
    name: 'App',
    mounted() {
        this.arrowKeyListeners();
    },
    props: ['id', 'disabled', 'value'],
    data() {
        return {
            address: this.value,
            currentItem: 0,
            suggestions: [],
            suggestionCount: 5
        }
    },
    methods: {
        arrowKeyListeners() {
            document.addEventListener('keyup', this.arrowNextItem);
            document.addEventListener('keyup', this.arrowSelectKey);
        },
        arrowNextItem(event) {
            if (event.keyCode == 38 && this.currentItem > 0) {
                this.currentItem--
            } else if (event.keyCode == 40 && this.currentItem < this.suggestions.length - 1) {
                this.currentItem++
            }
        },
        arrowSelectKey(event) {
            if (event.keyCode == 13) {
                if (this.suggestions.length > 0) {
                    this.selectAddress(this.suggestions[this.currentItem]);
                }
            }
        },
        autocomplete: _.debounce(function(event) {
            if (event.keyCode == 13) {
                return
            }

            axios.get('https://api.dataforsyningen.dk/autocomplete?fuzzy', {
                params: {
                    type: 'adresse',
                    q: this.address,
                    per_side: this.suggestionCount
                }
            }).then((res) => {
                this.suggestions = res.data
            })
        }, 500),
        selectAddress(suggestion) {
            this.address = this.suggestionText(suggestion)

            let streetnumber = suggestion.data.husnr
            streetnumber = suggestion.data.etage ? `${streetnumber}, ${suggestion.data.etage}.` : streetnumber
            streetnumber = suggestion.data.dør ? `${streetnumber} ${suggestion.data.dør}.` : streetnumber

            let data =  {
                streetname: suggestion.data.vejnavn,
                streetnumber: streetnumber,
                zipcode: suggestion.data.postnr,
                city: suggestion.data.postnrnavn,
                secondaryCity: suggestion.data.supplerendebynavn,
                completeAddress: this.suggestionText(suggestion)
            }

            this.suggestions = []

            this.currentItem = 0

            this.$emit('autocomplete', data)
        },
        suggestionText(suggestion) {
            return suggestion.tekst.replace(', ,', ',')
        }
    }
}
</script>

<style lang="css">
    .position-relative {
        position: relative;
    }

    .suggestion {
        border: none;
        cursor: pointer;
        user-select: none;
        border-radius: 0px;
    }

    .suggestion:hover {
        background-color: #16a085;
        color: #ecf0f1;
    }

    .active-item {
        background-color: #16a085;
        color: #ecf0f1;
    }

    #suggestion-wrapper {
        position: absolute;
        top: 100%;
        z-index: 7;
        width: 100%;
    }
</style>
