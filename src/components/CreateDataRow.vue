<template>
    <tr>
        <td><input v-model="fieldName" class="form-control" placeholder="New field name"/></td>
        <td>
            <select v-model="fieldType" class="form-control">
                <option v-for="(value, key) in dataTypes" v-bind:key="key">
                    {{ value }}
                </option>
            </select>
        </td>
        <td colspan="2">
            <button @click="$emit('add', addRow())" class="btn btn-dark">
                Add Field
            </button>
        </td>
    </tr>
    <tr v-if="success & !error">
        <td colspan="3">
            <div
                class="alert alert-success d-flex align-items-center justify-content-between"
                role="alert"
            >
                <div>Successfully added</div>
                <button class="btn btn-success" @click="closeBtn">Close</button>
            </div>
        </td>
    </tr>
    <tr v-if="!success & error">
        <td colspan="3">
            <div
                class="alert alert-danger d-flex align-items-center justify-content-between"
                role="alert"
            >
                <div>Failed to add</div>
                <button class="btn btn-alert" @click="closeBtn">Close</button>
            </div>
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
            error: false,
            success: false,
        };
    },
    // emit adding, editing and deleting
    methods: {
        closeBtn(){
            this.error = false;
            this.success = false;
        },
        addRow() {
            if (
                this.fieldName.length > 0 &&
                this.validateIfExisting(this.fieldName)
            ) {
                this.success = true;
                this.error = false;
                return {
                    fieldName: this.fieldName,
                    fieldType: this.fieldType,
                };
            }
            this.error = true;
            this.success = false;
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
