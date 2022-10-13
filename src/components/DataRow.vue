<template>
    <td>
        <p v-if="!editing">
            {{ row.fieldName }}
        </p>
        <input v-else v-model="newFieldName" />
    </td>
    <td>
        <p v-if="!editing">
            {{ row.fieldType }}
        </p>
        <select v-else v-model="newFieldType">
            <option v-for="(value, key) in dataTypes" v-bind:key="key">
                {{ value }}
            </option>
        </select>
    </td>
    <td v-if="!editing" colspan="2">
        <button @click="toggleEditing">Edit</button>
        <button @click="$emit('delete', deleteData())">Delete</button>
    </td>
    <td v-else>
        <button @click="$emit('save', saveData(),toggleEditing())">Save</button>
        <button @click="toggleEditing">Cancel</button>
    </td>
</template>
<script>
export default {
    name: "DataRow",
    props: ["row"],
    data() {
        return {
            dataTypes: ["string", "integer", "float", "boolean"],
            editing: false,
            // oldFieldName: this.row.fieldName,
            // oldFieldType: this.row.fieldType,
            newFieldName: this.row.fieldName,
            newFieldType: this.row.fieldType,
            errors: [],
        };
    },
    methods: {
        toggleEditing() {
            if (!(this.row.fieldName == "id")) {
                 this.errors = [];
                this.editing = !this.editing;
            }
        },
        saveData() {
            this.errors = [];
            if (!this.newFieldName) {
                this.errors.push("Field name cannot be empty");
            }
            if (this.newFieldName == "id") {
                this.errors.push("Forbidden field name");
            }
            if (this.errors.length == 0) {
                return {
                    oldFieldName: this.row.fieldName,
                    oldFieldType: this.row.fieldType,
                    newFieldName: this.newFieldName,
                    newFieldType: this.newFieldType,
                };
            }
        },
        deleteData() {
            this.errors = [];
            if (this.newFieldName == "id") {
                this.errors.push("Id cannot be deleted ");
                return null
            }
            if (this.errors.length == 0) {
                return {
                    fieldName: this.row.fieldName,
                    fieldType: this.row.fieldType,
                };
            }
        },
    },
};
</script>
