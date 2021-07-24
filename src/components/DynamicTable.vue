<template>


    <div class="d-flex mb-4">
        <input class="form-control me-2" type="text"  v-model="requestString" placeholder="Поиск">
        <button class="btn btn-primary" @click="search()">Поиск</button>
    </div>

    <table class="table table-bordered table-hover" v-if="content">
        <thead class="table-light">
            <tr>
                <th @click="alphabetSort()" class="pointer">Марка и модель {{ alphabetSortDirectionStraight ? "▼" : "▲"}}</th>
                <th v-for="tariff of content.tariffs_list" :key="tariff">
                    {{tariff}}
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="car of content.cars" :key="car">
                <td>{{car.mark}} {{car.model}}</td>
                <td 
                    v-for="tariff of content.tariffs_list" 
                    :key="tariff" 
                    :class="{pointer: car.tariffs[tariff]}"
                    @click="() => { car.tariffs[tariff] ? selectField(`${car.mark} ${car.model} ${car.tariffs[tariff]?.year}`) : void 0 }"
                >
                    {{car.tariffs[tariff]?.year || "-"}}
                </td>
            </tr>
        </tbody>
    </table>

    <div class="card bg-light mt-2 mb-2">
        <div class="card-body">
            {{selectedField ? `Выбран автомобиль ${selectedField} года выпуска` : `Выберите автомобиль и год выпуска`}} 
            <b v-if="selectedField" @click="selectField('')">[X]</b>
        </div>
    </div>
</template>

<style>
.table td:not(:first-child), th:not(:first-child) {
   text-align: center;   
}

.pointer{
    cursor: pointer;
}
</style>

<script>
export default {
    data(){
        return{
            content: null,
            contentCopy: null,
            dataChanged: false, // Были ли изменены данные
            alphabetSortDirectionStraight: true,
            requestString: "",
            selectedField: ""
        }
    },
    props: {
        contentProps: {
            type: Object,
            default: function () {
                return null
            }
        },
    },
    methods:{
        search(){

            this.content = this.dataChanged ? {...this.contentCopy} : this.content

            this.content.cars = this.content.cars.filter(car => {

                //Формируем массив carInfo

                const carInfoArr = [car.mark, car.model]

                for(const tariff of Object.values(car.tariffs))
                    carInfoArr.push(tariff.year.toString())
                
                //Формируем массив поискового запроса

                if(this.requestString.trim() === "") return true

                const requestArr = this.requestString.split(" ")

                // O(n^2) - плохо...было бы больше времени, подумал бы над более оптимальным решением
                for(const requestItem of requestArr)
                    if(!carInfoArr.includes(requestItem)) return false
                
                return true


                // if(!`${car.mark} ${car.model}`.includes(this.requestString)) return false
                // return true

            })

            this.dataChanged = true
        },
        selectField(str){
            this.selectedField = str
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
        //this.content = (await this.axios.get("https://city-mobil.ru/api/cars")).data
        this.content = {...this.contentProps}
        this.contentCopy = {...this.content}
    }
}
</script>