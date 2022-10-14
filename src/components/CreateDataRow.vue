<template>
    <tr>
        <td><input v-model="fieldName" class="form-control"/></td>
        <td>
            <select v-model="fieldType" class="form-control">
                <option v-for="(value, key) in dataTypes" v-bind:key="key">
                    {{ value }}
                </option>
            </select>
        </td>
        <td colspan="2">
            <button @click="$emit('add', addRow())" class="btn btn-dark">Add Field</button>
        </td>
    </tr>
</template>
<script>
export default {
    name: "CreateDataRow",
    props: ["fields"],
    data() {
        return {
            dataTypes: ["string", "integer", "float", "boolean"],
            fieldName: "",
            fieldType: "string",
            editing: false,
            error: false,
        };
    },
    // emit adding, editing and deleting
    methods: {
        addRow() {
            if (
                this.fieldName.length > 0 &&
                this.validateIfExisting(this.fieldName)
            ) {
                return {
                    fieldName: this.fieldName,
                    fieldType: this.fieldType,
                };
            }
            return null;
        },
        validateIfExisting(fieldName) {
            var duplicates = this.fields.filter(
                (field) => field.fieldName == fieldName
            );
            return duplicates.length == 0;
        },
    },
};
</script>
