<template>
    <div class = "wrapper">
        <h1>Яндекс Погода</h1>
        <p>
            Узнай какая погода сейчас в {{ city !== ''? cityName: 'твоём городе.' }}
        </p>
        <input type="text" v-model="this.city" placeholder="Введите город">
        <button v-show="city!==''" @click="getWeather()">Введите погоду</button>
        <template v-if="result != null">
            <p>{{ showTemp }}</p>
            <p>{{ showFellsLike }}</p>
            <p>{{ showMaxTemp }}</p>
            <p>{{ showMinTemp }}</p>
        </template>
    </div>
</template>

<script >
import axios from 'axios'
export default{
    data(){
        return{
            city:"",
            result:null,
            lat: null,
            lon: null
        }
    },
    methods:{
        getWeather(){
            axios.get(`https://api.openweathermap.org/geo/1.0/direct?q=${this.city}&limit=1&appid=db51f087327a0d83932e3647ba6b5035`)
            .then(res => {
                let coordinates = {}
                res = res.data[0]
                coordinates["lat"] = res["lat"]
                coordinates["lon"] = res["lon"]
                return coordinates
            }).then(coordinates=>{
                return axios.get(`https://api.openweathermap.org/data/2.5/weather?lat=${coordinates.lat}&lon=${coordinates.lon}&units=metric&appid=db51f087327a0d83932e3647ba6b5035`)
            }).then(answer =>{
                console.log(answer.data)
                this.result = answer.data
            })
        }
    },
    computed:{
        cityName(){
            return this.city + '!'
        },
        showTemp(){
            return `Температура сейчас: ${this.result.main.temp}°`
        },
        showFellsLike(){
            return `Ощущается как: ${this.result.main.feels_like}°`
        },
        showMinTemp(){
            return `Максимальная температура: ${this.result.main.temp_max}°`
        },
        showMaxTemp(){
            return `Минимальная температура: ${this.result.main.temp_min}°`
        }
    }
}
</script>

<style scoped>
.wrapper{
        width: 1000px;
        height: 600px;
        border-radius: 40px;
        background-color: rgb(245, 215, 181);
        padding: 20px;
        text-align: center;
        color:black;
        border: 1px solid seagreen;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        margin-top: 50px;

        box-shadow: rgba(0, 0, 0, 0.19) 0px 10px 20px, rgba(0, 0, 0, 0.23) 0px 6px 6px;
    input{
        margin-top: 40px;
        background: transparent;
        border:0;
        border-bottom: 1px solid green;
        color: darkorchid;
        font-size: 14px;
        text-shadow: 1px 1px 1px black;
        font-size: 12pt;
        padding: 5px 8px;
        outline:none;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        /*transition: property duration timing-function delay;*/
        transition: transform 300ms ease-out;
    }
    h1{
        font-weight: bold;
        font-size: 26pt;
        margin-top:20px;
    }
    p{
        font-size: 14pt;
        margin-top:20px;
    }
    input:focus{
        transform: scale(1.05) translateY(-5px);
        border-bottom: 2px solid black;
    }
    button{
        padding: 5px;
        margin-left: 15px;
        background-color: lightgoldenrodyellow;
        color:black;
        border: 1px solid black;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        transition:transform 400ms ease;
    }
    button:hover  {
        transform: scale(1.05) translateY(-5px);
        
        filter:brightness(80%);
    }
}


</style>
