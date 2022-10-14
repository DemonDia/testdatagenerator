<template>
    <td>
        <p v-if="!editing">
            {{ row.fieldName }}
        </p>
        <input v-else v-model="newFieldName" class="form-control"/>
    </td>
    <td>
        <p v-if="!editing">
            {{ row.fieldType }}
        </p>
        <select v-else v-model="newFieldType" class="form-control">
            <option v-for="(value, key) in dataTypes" v-bind:key="key">
                {{ value }}
            </option>
        </select>
    </td>
    <td v-if="!editing" colspan="2">
        <button @click="toggleEditing" class="btn btn-primary">Edit</button>
        <button @click="$emit('delete', deleteData())" class="btn btn-danger">Delete</button>
    </td>
    <td v-else>
        <button @click="$emit('save', saveData(), toggleEditing())" class="btn btn-primary">
            Save
        </button>
        <button @click="toggleEditing" class="btn btn-secondary">Cancel</button>
    </td>
</template>
<script>
export default {
    name: "DataRow",
    props: ["row", "fields"],
    data() {
        return {
            dataTypes: ["string", "integer", "float", "boolean"],
            editing: false,
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
            if (!this.validateIfExisting(this.newFieldName)) {
                this.errors.push("Field name exists");
            }
            console.log("errors", this.errors);
            if (this.errors.length == 0) {
                console.log({
                    oldFieldName: this.row.fieldName,
                    oldFieldType: this.row.fieldType,
                    newFieldName: this.newFieldName,
                    newFieldType: this.newFieldType,
                });
                return {
                    oldFieldName: this.row.fieldName,
                    oldFieldType: this.row.fieldType,
                    newFieldName: this.newFieldName,
                    newFieldType: this.newFieldType,
                };
            }
            return null;
        },
        deleteData() {
            this.errors = [];
            if (this.newFieldName == "id") {
                this.errors.push("Id cannot be deleted ");
                return null;
            }
            if (this.errors.length == 0) {
                return {
                    fieldName: this.row.fieldName,
                    fieldType: this.row.fieldType,
                };
            }
        },
        validateIfExisting(fieldName) {
            console.log("this.fields", this.fields);
            var duplicates = this.fields.filter(
                (field) => field.fieldName == fieldName
            );
            return duplicates.length == 0;
        },
    },
};
</script>
