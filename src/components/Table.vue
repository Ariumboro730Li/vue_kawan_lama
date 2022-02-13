<template>
    <tbody>
        <tr>
            <td>{{ id }}</td>
            <td>{{ title }}</td>
            <td>{{ imdbid }}</td>
            <td>
               <input type="checkbox" @click="checkMovie" :checked="isChecked">
            </td>
        </tr>
    </tbody>
</template>

<script>
export default {
    props: ['id', 'title', 'imdbid', 'isChecked'],
    methods: {
        checkMovie(event){
            this.$emit('checkChild', this.id, event.target.checked);
        }
    }
};
</script>
