<template>
    <div>
        <h1>Test Data Generator</h1>
        <GenerateJsonData :jsonData="fields" />
        <hr>
        <div id="main">
            <div id="dataGenTableContainer">
                <h4>Json table</h4>
                <table>
                    <tr>
                        <th>Field Name</th>
                        <th>Field Type</th>
                        <th colspan="2">Actions</th>
                    </tr>
                    <CreateDataRow
                        @add="(i) => addDataColumns(i)"
                        :fields="fields"
                    />
                    <tr v-for="(value, key) in fields" v-bind:key="key">
                        <DataRow
                            :row="value"
                            :fields="fields"
                            @save="(val) => saveChangedColumn(val)"
                            @delete="(val) => saveDeleteColumns(val)"
                        />
                    </tr>
                </table>
            </div>
            <div id="previewerContainer">
                <JsonPreview :jsonFields="{ fields }" />
            </div>
        </div>
    </div>
</template>

<script>
import CreateDataRow from "./components/CreateDataRow.vue";
import DataRow from "./components/DataRow.vue";
import JsonPreview from "./components/JsonPreview.vue";
import GenerateJsonData from "./components/GenerateJsonData.vue";
export default {
    name: "App",
    components: { CreateDataRow, DataRow, JsonPreview, GenerateJsonData },
    data() {
        return {
            fields: [
                {
                    fieldName: "id",
                    fieldType: "integer",
                },
            ],
        };
    },
    methods: {
        saveChangedColumn(column) {
            console.log("changedCol", column);
            if (column) {
                this.fields = this.fields.map((field) => {
                    console.log("field", field);

                    if (
                        field.fieldName == column.oldFieldName &&
                        field.fieldType == column.oldFieldType
                    ) {
                        return {
                            fieldName: column.newFieldName,
                            fieldType: column.newFieldType,
                        };
                    } else {
                        return field;
                    }
                });
            }
        },
        saveDeleteColumns(column) {
            this.fields = this.fields.filter(
                (field) =>
                    !(
                        field.fieldName == column.fieldName &&
                        field.fieldType == column.fieldType
                    ) && field
            );
        },
        addDataColumns(column) {
            if (column) {
                this.fields.push(column);
            }
        },
    },
    watch: {
        fields: function (val) {
            this.fields = val;
        },
    },
    // only responsible for exporting & previewing
};
</script>
<style>
html {
    background: grey;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
    background: grey;
}
#previewerContainer,
#dataGenTableContainer,
#generatorContainer {
    padding: 10px;
}
h1,
h2,
h3,
h4,
h5,
h6,
label,
th,
td,hr {
    color: white;
}
#dataGenTableContainer {
    width: 60%;
}
#previewerContainer {
    width: 40%;
}
#main {
    display: flex;
}
table {
    border-radius: 5px;
    width: 100%;
    background: black;
}

td {
    text-align: center;
    padding: 5px;
}
td p {
    margin: auto;
}
/* table{
    width: 100%;
} */
</style>
