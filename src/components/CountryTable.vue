<template>
    <div class="container mt-5">
        <loader v-if="!isLoad"></loader>
        <div v-else class="container-fluid">
            <div class="row">
                <div class="col-6">
                    <div class="form-group mb-3">
                        <label for="exampleInputEmail1">Capital Filter:</label>
                        <input v-model="searchString" type="text" class="form-control" id="exampleInputEmail1">
                        <small id="emailHelp" class="form-text text-muted">Please write the capital you want to search</small>
                    </div>
                </div>
                <div class="col-6">
                    <div class="form-group mb-3">
                        <label for="exampleInputEmail1">General Filter:</label>
                        <input v-model="generalFilterString" type="text" class="form-control" id="exampleInputEmail1">
                        <small id="emailHelp" class="form-text text-muted">Please write the capital you want to search</small>
                    </div>
                </div>
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
            searchString:"",
            generalFilterString:"",
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
       setUndefinedRegionalBlocks(){
           for(let i=0;i<this.countryDatas.length;i++){
               if(this.countryDatas[i].regionalBlocs==undefined){
                   this.countryDatas[i].regionalBlocs=[];
               }
           }
       },
       async getCountries(){
           try{
            let {data}=await axios.get("https://restcountries.com/v2/all");
            this.countryDatas=data;
            this.setUndefinedCapitals();
            this.setUndefinedRegionalBlocks();
            this.isLoad=true;
           }catch(err){
            alert("Lütfen internet bağlantınızı kontrol ediniz!");
           }
       }
    },
    computed:{
       filteredDatas(){
               if(this.generalFilterString.length>0){
                   return this.countryDatas.filter(el=>{
                       if(el.capital.toLocaleLowerCase().includes(this.generalFilterString) || el.name.toLocaleLowerCase().includes(this.generalFilterString) || el.region.toLocaleLowerCase().includes(this.generalFilterString) || el.subregion.toLocaleLowerCase().includes(this.generalFilterString) || el.demonym.toLocaleLowerCase().includes(this.generalFilterString) ){
                           return true;
                       }
                       else{
                           return false;
                       }
                       
                   });
               }
               else{
                    return this.countryDatas.filter((country)=>{
                        return country.capital.toLocaleLowerCase().includes(this.searchString.toLocaleLowerCase())
                    })
               }
              
           
           
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