<template>
    <div class="mt-4">
        <div class="card text-left">
            <div class="card-body">
                <div class="row">
                    <div class="col-6">
                        <h4 class="card-title mt-3">Row Checked : <span>{{checkedCount}}</span></h4>
                    </div>
                    <div class="col-6">
                        <div class="form-group">
                            <label for="">Year of Movie</label>
                            <select class="form-control" name="" @change="selectYearofMovie" id="">
                                <option v-for="year in yearsOfMovie" :key="year.year">{{year.year}}</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <the-child :yearOfMovie="yearOfMovie" @counterCheck="checkedCount = $event"></the-child>
</template>
<script>
import TheChild from "./Child.vue";
export default {
    data(){
        return {
            checkedCount: 0,
            yearsOfMovie : [],
            yearOfMovie : 2010,
        }
    },
    components: {
        TheChild
    },
    methods: {
        selectYearofMovie(event){
            this.yearOfMovie = event.target.value;
        },
        loadYearsOfMovie(){
            const options = [];
            for (let index = new Date().getFullYear() - 15 ; index <= new Date().getFullYear() ; index++) {
                options.push({
                    year : index
                });
            }
            this.yearsOfMovie = options;
        },
    },
    mounted(){
        this.loadYearsOfMovie();
    }
};
</script>
