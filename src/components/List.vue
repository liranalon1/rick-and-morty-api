<template>
    <div id="list">
        <div class="container">
            <h2>The Most unpopular character from Earth C-137</h2>
            <div class="main-table">
                <ul class="flex table-row table-header">
                    <li v-for="(item, index) in props.labels" :key="index">
                    {{item}}
                    </li>
                </ul>

                <div class="table-collection">
                    <ul class="flex table-row row-collection">
                        <li>{{props.unpopularCharacter.name}}</li>
                        <li>{{props.unpopularCharacter.origin.name}}</li>
                        <li>{{props.unpopularCharacter.location.name}}</li>
                        <li>{{props.unpopularCharacter.episode.length}}</li>
                    </ul>
                </div>
            </div>        
        </div>
    </div>
</template>
    
<script setup>
    import { computed, reactive } from 'vue'

    const props = defineProps({
        unpopularCharacter: Array,
        labels: Array,
    });

</script>

<style lang="scss" scoped>
    @import "../assets/scss/global.scss";
    @import "../assets/scss/variables.scss";

    #list{
        margin: 20px auto;
        padding: 0 20px;

        .container{
            position: relative;
            max-width: 1000px;
            margin: 0 auto;
        }
    
        .table-row{
            justify-content: space-between;
            border: 1px solid #4e4e4e;
            text-align: center;
            &:nth-child(even){
                background-color: #efefef;
            }
        }
        .table-collection{
            max-height: 500px;
            overflow-x: auto;            

            .row-collection{
                transition: 0.2s;

                &:nth-child(2){
                    border-top-left-radius: 5px;
                    border-top-right-radius: 5px;
                }
                &:last-of-type{
                    border-bottom-right-radius: 5px;
                    border-bottom-left-radius: 5px;
                }
            }            
        }
        
        .table-header{
            font-size: 18px;
            font-weight: 700;
            border: none;
            
            li{
                text-transform: capitalize;
            }
        }
        ul{
            li{
                flex: 1;
                padding: 10px;
            }
        }
    }

    @media (max-width: 640px) {
        #list {
            ul{
                li{
                    flex: 1;
                    padding: 5px;
                    font-size: 14px;
                }
            }
        }
    }  
</style>