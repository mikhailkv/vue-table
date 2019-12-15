<template >
<tr>
    <td 
        @dblclick="editCell(row[key])" 
        v-for="key in fields" v-bind:key="key.id"
    >
        <span v-if="!row[key].isEdited"> {{ row[key].value }}</span>
        <input 
            v-else 
            @blur="row[key].isEdited=false"
            @keyup.enter="row[key].isEdited=false"
            @keyup.esc="resetCell(row[key])" 
            type="text" 
            v-model="row[key].value">
    </td>
</tr>
</template>

<script>
export default {
    name: 'TableRow',
    props: {
        row: {
            type: Array
        },
        fields: {
            type: Array
        }
    },
    data(){
        return{
            cellData: ''
        }
    },
    methods: {
        editCell: function(cell){
            this.cellData = cell.value
            cell.isEdited=true
        },
        resetCell: function(cell){
            cell.isEdited=false
            return (this.cellData) ? cell.value=this.cellData: cell.value;

        }
            
    }

}
</script>

<style scoped>

</style>
