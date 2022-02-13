<template>  
    <h1 v-if="isLoading">Loading....</h1>
    <h1 v-else-if="messageError"> {{messageError}} </h1>
    <h1 v-else-if="results.length == 0">NO DATA</h1>
    <table v-else class="table">
        <thead>
            <tr>
                <td>Id</td>
                <td>Title</td>
                <td>ImdbID</td>
                <td>
                    <input type="checkbox" @click="checkAllMovie" :checked="isAllChecked">
                </td>
            </tr>
        </thead>
        <the-body v-for="result in results"
            :key="result.id" 
            :id="result.id"       
            :title="result.title"       
            :imdbid="result.imdbid"       
            :isChecked="result.isChecked"    
            @checkChild="countChildChecked"   
        ></the-body>
    </table>
</template>

<script>
import TheBody from "./Table.vue";
export default {
    props: ["currentYear"],
    components: {
        TheBody,
    },
    data() {
        return {
            results: [],
            isLoading: false,
            messageError : null,
            isAllChecked : false,
            presYear : this.currentYear,
            childChecked : [],
        }
    },
    methods: {
        loadExperiences(){
            this.isLoading = true;
            this.$emit('counterCheck',0);
            fetch(`https://jsonmock.hackerrank.com/api/moviesdata?Year=${this.presYear}`).then(
                (response) => {
                    if (response.ok) {
                        return response.json();
                    }
                }
            ).then((data) => {
                const results = [];
                for (const id in data){
                    let datas = data[id]
                    for (const key in datas) {
                        results.push({
                            id : parseInt(key) + 1,
                            title : datas[key].Title,
                            imdbid : datas[key].imdbID,
                            isChecked : false
                        })
                    }
                }
                this.results = results;
                this.isLoading = false;
            }).catch((error) => {
                this.isLoading = false;
                this.messageError = `${error}. Please Try Again`;
            });
        },
        checkAllMovie(){
            this.isAllChecked = !this.isAllChecked
        },
        countChildChecked(id, value){
            const result = this.childChecked;
            if (value) {
                result.push(id);
            } else {
                console.log(value);
                const index = result.indexOf(id);
                if (index !== -1) {
                    result.splice(index, 1);
                }
            }
            this.childChecked = result;
            this.$emit('counterCheck', this.childChecked.length);
            if (this.childChecked.length <= 0) {
                this.isAllChecked = false;
            }
        }
    },
    watch: {
        isAllChecked() {
            const results = [];
            const data = this.results;
            this.childChecked = [];
            for (const id in data){
                let datas = data[id];
                results.push({
                    id : parseInt(id) + 1,
                    title : datas.title,
                    imdbid : datas.imdbid,
                    isChecked :this.isAllChecked,
                })
                if (this.isAllChecked == true) {
                    this.childChecked.push(parseInt(id) + 1);
                }
            }
            this.results = results;
            this.isLoading = false;
            this.$emit('counterCheck', (this.isAllChecked)? results.length : 0);
        },
        currentYear(value){
            this.presYear = value;
            this.childChecked = [];
            this.loadExperiences();
        },
        childChecked(){
        }
    },
    mounted() {
        // this.loadExperiences();

    },
};
</script>
