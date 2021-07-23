<template>

    <input type="text" v-model="requestString">
    <button @click="search()">Поиск</button>

    <table border="1" v-if="content">
        <tr>
            <td @click="alphabetSort()">Марка и модель {{ alphabetSortDirectionStraight ? "&#129083;" : "&#129081;"}}</td>
            <td v-for="tariff of content.tariffs_list" :key="tariff">{{tariff}}</td>
        </tr>
        <tr v-for="car of content.cars" :key="car">
            <td>{{car.mark}} {{car.model}}</td>
            <td v-for="tariff of content.tariffs_list" :key="tariff">
                {{car.tariffs[tariff]?.year || "-"}}
            </td>
        </tr>
    </table>
</template>

<script>
export default {
    data(){
        return{
            content: null,
            contentCopy: null,
            dataChanged: false, // Были ли изменены данные
            alphabetSortDirectionStraight: true,
            requestString: ""
        }
    },
    methods:{
        search(){

            //Допилить возможность искать с годами, типа Audi 2013
            //Переводить задачу в область поиска подпоследовательности в массиве

            this.content = this.dataChanged ? {...this.contentCopy} : this.content

            this.content.cars = this.content.cars.filter(car => {
                if(!`${car.mark} ${car.model}`.includes(this.requestString)) return false
                return true
            })

            this.dataChanged = true
        },
        alphabetSort(){
            this.content.cars.sort((a, b) => {

                const   first =  `${a.mark} ${a.model}`.toLowerCase(),
                        second = `${b.mark} ${b.model}`.toLowerCase()

                if(this.alphabetSortDirectionStraight){

                    if(first > second) { return -1; }
                    if(first < second) { return 1; }
    

                }else{

                    if(first < second) { return -1; }
                    if(first > second) { return 1; }

                }

                return 0;
            })

            this.alphabetSortDirectionStraight = !this.alphabetSortDirectionStraight
            this.dataChanged = true

        }  
    },
    async created(){
        this.content = (await this.axios.get("https://city-mobil.ru/api/cars")).data
        this.contentCopy = {...this.content}
    }
}
</script>