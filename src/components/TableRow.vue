<template >
<tr>
    <td 
        @dblclick="editCell(rowLocal[key])" 
        v-for="key in fieldsLocal" v-bind:key="key.id"
    >
        <span v-if="!rowLocal[key].isEdited"> {{ rowLocal[key].value }}</span>
        <input 
            v-else 
            @blur="rowLocal[key].isEdited=false"
            @keyup.enter="rowLocal[key].isEdited=false"
            @keyup.esc="resetCell(rowLocal[key])" 
            type="text" 
            v-model="rowLocal[key].value"
            ref="input">
    </td>
</tr>
</template>

<script>
export default {
    name: 'TableRow',
    props: {
        row: {
            type: Object
        },
        fields: {
            type: Array
        }
    },
    data(){
        return{
            cellData: '',
            rowLocal: this.util.extend({}, this.row),
            fieldsLocal: [...this.fields]
        }
    },
    methods: {
        editCell: function(cell){
            this.cellData = cell.value
            cell.isEdited=true
            this.$nextTick(()=> {
                if (this.$refs.input) {
                    this.$refs.input[0].focus()
                }
            })
            
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
