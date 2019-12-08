<template>
<div class="hello">
    <div v-if="loading" class="progress">
        <div class="indeterminate"></div>
    </div>
    <div v-else>
        <input v-model="search" type="text">
        <table class="responsive-table">
            <thead>
                <tr>
                    <th v-for="key in fields" v-bind:key="key.id">
                        <a href="#" v-on:click="sortBy(key)">
                            {{ key }}
                        </a>
                    </th>
                </tr>
            </thead>
            <tbody v-for="row in sortRows" v-bind:key="row.id">
                <tr>
                    <td v-for="key in fields" v-bind:key="key.id">
                        <span> {{ row[key] }}</span>
                        <input type="text" value="">
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</div>
</template>

<script>
import 'materialize-css/dist/css/materialize.css'
import axios from 'axios'

export default {
    name: 'VueTable',
    data() {

        return {
            data: [],
            loading: false,
            fields: [],
            rows: [],
            search: '',
            sortKey: '',
            reverse: false
        }
    },
    methods: {
        sortBy: function (sortKey) {
            this.reverse = (this.sortKey == sortKey) ? !this.reverse : false;
            this.sortKey = sortKey;
        },
        // Получаем заголовок
        getFields(data) {
            this.fields = Object.keys(data)
        },
        // Агрегируем полученную data в массиве
        getRows(data) {
            data.forEach(element => {
                let row = {}
                this.getRow(element, row)
                this.rows.push(row)
            });
        },
        // Получаем строку рекурсивно
        // обходя все вложенные элементы 
        getRow(data, row, parent = null) {
            let value;
            let parentKey = (parent) ? `${ parent }.` : ''
            for (let key in data) {
                value = data[key]
                key = `${ parentKey }${ key }`
                if (Object.prototype.toString.call(value) === '[object Object]') {
                    this.getRow(value, row, key)
                } else {
                    row[key] = value
                }
            }
            return
        },
        compareValues(key, reverse) {
            return function innerSort(a, b) {
                if (!a.hasOwnProperty(key) || !b.hasOwnProperty(key)) {
                    return 0;
                }
                const varA = (typeof a[key] === 'string') ?
                    a[key].toUpperCase() : a[key];
                const varB = (typeof b[key] === 'string') ?
                    b[key].toUpperCase() : b[key];

                let comparison = 0;
                if (varA > varB) {
                    comparison = 1;
                } else if (varA < varB) {
                    comparison = -1;
                }
                return (
                    (reverse) ? (comparison * -1) : comparison
                );
            };
        },
        searchValue(el) {
            if (this.search) {

                let values = Object.values(el)
                for (let i = 0; i < values.length; i++) {
                    if (values[i].toString().includes(this.search)) {
                        return el
                    }
                }

            } else {
                return el
            }

        }
    },
    mounted() {
        this.loading = true
        axios({
                method: 'get',
                url: 'http://www.filltext.com/?rows=10&id={number|1000}&firstName={firstName}&lastName={lastName}&email={email}&phone={phone|(xxx)xxx-xx-xx}&adress={addressObject}&description={lorem|32}'
            })
            .then(response => {
                this.data = response.data;
                this.loading = false
                this.getRows(this.data)
                this.getFields(this.rows[0])
            })

    },
    props: {
        msg: String
    },
    computed: {
        sortRows: function () {
            return [...this.rows].filter(this.searchValue)
                .sort(this.compareValues(this.sortKey, this.reverse))
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped>
h3 {
    margin: 40px 0 0;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    display: inline-block;
    margin: 0 10px;
}

a {
    color: #42b983;
}

table {
    width: 100%;
    /* Ширина таблицы */
    border: 4px double black;
    /* Рамка вокруг таблицы */
    border-collapse: collapse;
    /* Отображать только одинарные линии */
}

th {
    text-align: left;
    /* Выравнивание по левому краю */
    background: #ccc;
    /* Цвет фона ячеек */
    padding: 5px;
    /* Поля вокруг содержимого ячеек */
    border: 1px solid black;
    /* Граница вокруг ячеек */
}

td {
    padding: 5px;
    /* Поля вокруг содержимого ячеек */
    border: 1px solid black;
    /* Граница вокруг ячеек */
}
</style>
