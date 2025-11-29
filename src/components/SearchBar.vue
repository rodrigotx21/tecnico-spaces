<script>
import { nextTick } from 'vue';
import '@material/web/chips/filter-chip.js';
import '@material/web/chips/chip-set.js';
import debounce from 'lodash.debounce';

export default {
    name: 'SearchBar',
    data() {
        return {
            searchQuery: '',
            selectedType: '',
        };
    },
    emits: ['search'],
    methods: {
        onSearch() {
            // Emit the search query and selected type
            this.$emit('search', this.searchQuery, this.selectedType);
        },
        debounceSearch() {
            if (!this.debouncedSearch) {
                this.debouncedSearch = debounce(() => {
                    this.onSearch();
                }, 300); // Adjust debounce delay as needed
            }
            this.debouncedSearch();
        },
        toggleType(type) {
            this.selectedType = this.selectedType === type ? '' : type;
            
            // Use nextTick to ensure DOM updates before calling debounced search
            nextTick(() => {
                this.debounceSearch();
            });
        }
    }
}
</script>

<template>
    <section class="SearchHeader">
        <div class="SearchBar">
            <svg class="icon" xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#303241"><path d="M784-120 532-372q-30 24-69 38t-83 14q-109 0-184.5-75.5T120-580q0-109 75.5-184.5T380-840q109 0 184.5 75.5T640-580q0 44-14 83t-38 69l252 252-56 56ZM380-400q75 0 127.5-52.5T560-580q0-75-52.5-127.5T380-760q-75 0-127.5 52.5T200-580q0 75 52.5 127.5T380-400Z"/></svg>
            <input 
                v-model="searchQuery" 
                @input="debounceSearch" 
                type="search" 
                name="search" 
                id="search" 
                placeholder="Pavilhão Central" 
                autocomplete="off"
                pattern="\S+.*">
        </div>
        <!--<md-chip-set>
            <md-filter-chip 
                label="Campus" 
                @click="toggleType('CAMPUS')" 
                :selected="selectedType == 'CAMPUS'">
            </md-filter-chip>
            <md-filter-chip 
                label="Building" 
                @click="toggleType('BUILDING')" 
                :selected="selectedType == 'BUILDING'">
            </md-filter-chip>
            <md-filter-chip 
                label="Floor" 
                @click="toggleType('FLOOR')" 
                :selected="selectedType == 'FLOOR'">
            </md-filter-chip>
            <md-filter-chip 
                label="Room" 
                @click="toggleType('ROOM')" 
                :selected="selectedType == 'ROOM'">
            </md-filter-chip>
        </md-chip-set>-->
    </section>
</template>

<style scoped>
    .SearchHeader {
        background: var(--background-color);
        display: flex;
        flex-direction: column;
    }
    .SearchBar {
        margin: 1rem 0.625rem;
        display: flex;
        align-items: center;
        border-radius: 1.75rem;
        padding: 0.5rem 1rem;
        background: var(--card-color);
        transition: all 300ms ease-in-out;
        box-shadow: 4px 8px 8px rgba(0, 0, 0, 0.15);
    }
    .SearchHeader:focus-within .SearchBar {
        margin: 1rem 0;
        background: transparent;
        border-radius: 0;
        border-bottom: 2px solid var(--text-color);
        transition: all 300ms;
    }

    /* md-chip-set {
        display: none;
        flex-wrap: nowrap;
        padding: 0 0.625rem 1rem 0.625rem;
        width: 100%;
        height: fit-content;
        overflow-x: auto;
    }

    .SearchHeader:focus-within md-chip-set,
    .SearchBar:has(input:not(:placeholder-shown)) + md-chip-set,
    md-chip-set:has(md-filter-chip[selected]) {
        display: flex;
        gap: 0.5rem;
    } */

    .label {
        --label-text-font: Inter;
        color: var(--text-color);
    }

    svg.icon {
        width: 1.5rem;
        height: 1.5rem;
        margin: 0.5rem;
    }

    input[type="search"] {
        width: 100%;
        border: none;
        background: none;
        color: var(--text-color);
        font-size: 1rem;
    }
    input[type="search"]:focus {
        outline: none;
    }
    input[type="search"]::-webkit-search-cancel-button {
        -webkit-appearance: none;
        appearance: none;
    }
</style>
