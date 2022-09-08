<template>
    <div id="barChart">
        <h2>Popularity</h2>
        <div class="progress-bar-wrap">
            <div class="progress-bar" v-for="(character, index) in props.characters" :key="index">
                <div class="progress-track">
                <div class="progress-fill" :style="{height: (character.popularity > 100 ? 100 : character.popularity+'%'), backgroundColor: character.legendColor}">
                    <span>{{character.popularity}}</span>
                </div>
                </div>
            </div>

            <ul class="legend flex">
                <li class="flex" v-for="(character, index) in props.characters" :key="index">
                    <div class="square" :style="{backgroundColor: character.legendColor}"></div>
                    <div class="name">{{character.name}}</div>
                </li>
            </ul>
        </div>        
    </div>
</template>
    
<script setup>
  import { computed, reactive } from 'vue'

    const props = defineProps({
        characters: Array,
    }) 
</script>

<style lang="scss" scoped>
    @import "../assets/scss/global.scss";
    @import "../assets/scss/variables.scss";

    #barChart{
        .progress-bar-wrap {
            display: flex;
            gap: 25px;
            width: fit-content;
            margin: 20px auto;
            background: #fff;
            overflow: hidden;

            .progress-track, 
            .progress-fill {
                border-radius: 3px;
            }


            .progress-bar {
                height: 300px;
                width: 40px;
            }

            .progress-track {
                position: relative;
                width: 40px;
                height: 100%;
                background: #ebebeb;
            }

            .progress-fill {
                position: absolute;
                background: $primary-color;
                height: 50%;
                width: 40px;
                color: #fff;
                text-align: center;
                font-size: 12px;
                line-height: 20px;
                bottom: 0;
            }            
        }

        .legend{
            flex-direction: column;
            gap: 10px;
            li{
                gap: 10px;
                .square{
                    width: 24px;
                    height: 24px;
                }
            }
        }
    }

    @media (max-width: 640px) {

    }  
</style>