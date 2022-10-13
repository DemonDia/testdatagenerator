<template>
    <tr>
        <td><input v-model="fieldName" /></td>
        <td>
            <select v-model="fieldType">
                <option v-for="(value, key) in dataTypes" v-bind:key="key">
                    {{ value }}
                </option>
            </select>
        </td>
        <td colspan="2">
            <button @click="$emit('add', addRow(), reset())">Add Field</button>
        </td>
    </tr>
</template>
<script>
export default {
    name: "CreateDataRow",
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
        reset() {
            console.log("reset");
            (this.fieldName = ""), (this.fieldType = "string");
        },
        addRow() {
            console.log("fieldName", this.fieldName);
            if (this.fieldName.length > 0) {
                return {
                    fieldName: this.fieldName,
                    fieldType: this.fieldType,
                };
            }
            return null
        },
    },
};
</script>
