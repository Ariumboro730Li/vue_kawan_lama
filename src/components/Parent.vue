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
                            <select class="form-control" name="" @change="selectcurrentYear" id="">
                                <option v-for="year in yearsOfMovie" :key="year.year" :selected="year.selected">{{year.year}}</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <the-child :currentYear="currentYear" @counterCheck="checkedCount = $event" ref="child"></the-child>
</template>
<script>
import TheChild from "./Child.vue";
export default {
    data(){
        return {
            checkedCount: 0,
            yearsOfMovie : [],
            currentYear : 2015,
        }
    },
    components: {
        TheChild
    },
    methods: {
        selectcurrentYear(event){
            this.currentYear = event.target.value;
        },
        loadYearsOfMovie(){
            const options = [];
            // const randNum = new Date().getFullYear() - Math.floor(Math.random() * 15);
            const randNum = this.currentYear;
            for (let index = new Date().getFullYear() - 15 ; index <= new Date().getFullYear() ; index++) {
                let selected;
                if (randNum == index){
                    selected = "selected";
                    this.currentYear = index;
                } 
                options.push({
                    year : index,
                    selected : selected
                });
            }
            this.yearsOfMovie = options;
            this.$refs.child.loadExperiences();
        },
    },
    mounted(){
        this.loadYearsOfMovie();
    }
};
</script>
