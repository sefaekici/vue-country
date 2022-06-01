<template>
    <div class="container mt-5">
        <loader v-if="!isLoad"></loader>
        <div v-else class="container-fluid">
             <div class="form-group mb-3">
                <label for="exampleInputEmail1">Capital Filter:</label>
                <input v-model="searchString" type="text" class="form-control" id="exampleInputEmail1">
                <small id="emailHelp" class="form-text text-muted">Please write the capital you want to search</small>
            </div>
            <table class="table" >
            <thead>
                <tr>
                    <th scope="col">#</th>
                    <th>Name</th>
                    <th>Capital</th>
                    <th>Region</th>
                    <th>Flag</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(country,index) in filteredDatas" :key="index">
                    <th scope="row">{{index+1}}</th>
                    <td>{{country.name}}</td>
                    <td>{{country.capital}}</td>
                    <td>{{country.region}}</td>
                    <td><img class="country-flag" :src="country.flags.png" :alt="'img-alt' + country.name"></td>
                </tr>
            </tbody>
        </table>
        </div>
        
    </div>
</template>


<script>
import axios from "axios";
import loader from "./Loader.vue";
export default {
    data(){
        return{
            countryDatas:[],
            isLoad:false,
            searchString:""
        }
    },
    components:{
        loader,
    },
    methods:{
       setUndefinedCapitals(){
            for(let i=0;i<this.countryDatas.length;i++){
                if(this.countryDatas[i].capital==undefined){
                    this.countryDatas[i].capital="-";
                }
            }
       },
       async getCountries(){
           try{
            let {data}=await axios.get("https://restcountries.com/v2/all");
            this.countryDatas=data;
            this.setUndefinedCapitals();
            this.isLoad=true;
           }catch(err){
            alert("Lütfen internet bağlantınızı kontrol ediniz!");
           }
       }
    },
    computed:{
       filteredDatas(){
           return this.countryDatas.filter((country)=>{
                    return country.capital.toLowerCase().includes(this.searchString.toLowerCase())
           })
       }
    },
    async mounted(){
        await this.getCountries();
    }
}
</script>


<style lang="scss" scoped>
    .country-flag{
        width: 60px;
    }
</style>