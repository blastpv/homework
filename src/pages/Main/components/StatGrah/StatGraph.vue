<template>
    <div>
        <form class="form-settings">
            <!-- <div class="settings-graph-control">
                <label for="">Увеличение:</label>
                <input type="number" v-model="scaleY" />
            </div> -->
            <div class="settings-graph-control">
                <label for="">Увеличение:</label>
                <vue-numeric-input  v-model="scaleY" :min="1" :max="100" :step="1"></vue-numeric-input>
            </div>
            <!-- <div class="settings-graph-control">
                <label for="">Ширина колонки:</label>
                <input type="number" v-model="widthColumn" />
            </div> -->
            <div class="settings-graph-control">
                <label for="">Ширина колонки:</label>
                <vue-numeric-input  v-model="widthColumn" :min="1" :max="100" :step="1"></vue-numeric-input>
            </div>
            <div class="settings-graph-control">
                <select v-model="graphFilter">
                    <option :value="null" selected>Все значения</option>
                    <option :value="100">> 100</option>
                    <option :value="300">> 300</option>
                    <option :value="1000">> 1000</option>
                    <option :value="2000">> 2000</option>
                </select>
            </div>
        </form>
        <div>
            <vue-range-slider ref="slider" v-model="rengeValue"></vue-range-slider>
        </div>
        <div class="stat-graph">
            <!-- ДЗ: country.active / 5000) * scaleY вынести в computed -->
            <div 
                class="stat-graph-column"
                v-for="(country, index) in filtredData"
                :key="`${country.name}_${index}`"
                :style="{ height: `${((country.active / scaleY))}px`, width: `${widthColumn}px`, zoom: `${rengeValue}%`}"
                @mousemove="handleHover(index)"
            >
            <!--  -->
                <div class="column-info" v-if="hoveredIndex === index" :style="columnInfoStyle">
                    <div class="column-info-row">
                        <img :src="require('../../../../assets/icons/calendar.svg')" alt="calendar">
                        <span>{{country.date.slice(0,10)}}</span>
                    </div>
                    <div class="column-info-row">
                        <img :src="require('../../../../assets/icons/active.svg')" alt="">
                        <span>Кол.во заболевших: {{country.active}}</span>
                    </div>
                    <div class="column-info-row">
                        <img :src="require('../../../../assets/icons/recovered.svg')" alt="">
                        <span>Кол.во выздоровевших: {{country.recovered}}</span>
                    </div>
                </div>
            </div>
        </div>
            <!-- <div class="info" v-if="dataCountry.length > 0 && !!hoveredIndex">
                <div class="info-row">
                    <img :src="require('../../../../assets/icons/calendar.svg')" alt="calendar">
                    <span>{{filtredData[hoveredIndex].date}}</span>
                </div>
                <div class="info-row">
                    <img :src="require('../../../../assets/icons/active.svg')" alt="">
                    <span>Кол.во заболевших: {{filtredData[hoveredIndex].active}}</span>
                </div>
                <div class="info-row">
                    <img :src="require('../../../../assets/icons/recovered.svg')" alt="">
                    <span>Кол.во выздоровевших: {{filtredData[hoveredIndex].recovered}}</span>
                </div>
            </div> -->
    </div>
</template>
<script>

import 'vue-range-component/dist/vue-range-slider.css'
import VueRangeSlider from 'vue-range-component'
import VueNumericInput from 'vue-numeric-input'

export default {
    name: "StatGraph",

    props: {
        dataCountry: Array
    },

    data() {
        return {
            scaleY: 5,
            widthColumn: 20,
            graphFilter: null,
            hoveredIndex: null,
            rengeValue: 100,
        }
    },

    components: {
        VueRangeSlider,
        VueNumericInput
    },

    methods: {
        handleHover(index) {
            this.hoveredIndex = index;
            // console.log(this.hoveredIndex);
        }
    },

    computed: {
        filtredData() {
            return this.dataCountry.filter((country) => country.active >= this.graphFilter)
        },

        columnInfoStyle() {
            if (this.hoveredIndex > 225) {
                return { right: '1px' }
            } else { 
                return { left: '1px' }
            }
        },
        // не получается передать значение
        // countryActiveMath(){
        //         return (this.filtredData[this.hoveredIndex].active /5000) * this.scaleY
        // },

        changeRengeValue(){
            return this.rengeValue
        }
    },
}
</script>
<style scoped>
.stat-graph {
    height: 500px;
    width: 100%;
    overflow-x: hidden;
    border: 1px solid #999999;
    display: flex;
    justify-content: flex-start;
    align-items: flex-end;
}

.stat-graph-column {
    position: relative;
    cursor: pointer;
    margin-right: 1px;
    background-color: rgb(102, 119, 214);
}

.stat-graph-column:hover {
    background-color: rgb(226, 90, 90);
}

.form-settings {
    display: flex;
    margin-bottom: 30px;
}

.settings-graph-control {
    margin-right: 30px;
    font-size: 18px;
}

.column-info {
    position: absolute;
    min-width: 250px;
    z-index: 100;
    background-color: #333333;
    border-radius: 5px;
    padding: 5px;
    font-size: 12px;
    color: #999999;
    bottom: 80px;
}

.column-info-row {
    display: flex;
    align-items: center;
}

.column-info-row img {
    margin-right: 10px;
    width: 15px;
}

.info {
    max-width: 350px;
    z-index: 100;
    background-color: #333333;
    border-radius: 5px;
    padding: 5px;
    font-size: 18px;
    color: #999999;
    left: 1px;
    bottom: 80px;
}

.info-row {
    display: flex;
    align-items: center;
}

.info-row img {
    margin-right: 10px;
    width: 20px;
}
</style>