<template>
    <tr v-if="success & errors.length==0">
        <td colspan="3">
            <div
                class="alert alert-success d-flex align-items-center justify-content-between"
                role="alert"
            >
                <div>Successfully updated</div>
                <button class="btn btn-success" @click="closeBtn">Close</button>
            </div>
        </td>
    </tr>
    <tr v-if="!success & errors.length>0">
        <td colspan="3">
            <div
                class="alert alert-danger d-flex align-items-center justify-content-between"
                role="alert"
            >
                <div>{{ this.errors }}</div>
                <button class="btn btn-alert" @click="closeBtn">Close</button>
            </div>
        </td>
    </tr>
    <tr>
    <td>
        <p v-if="!editing">
            {{ row.fieldName }}
        </p>
        <input v-else v-model="newFieldName" class="form-control" />
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
        <button
            v-if="this.row.fieldName != 'id'"
            @click="toggleEditing"
            class="btn btn-primary"
        >
            Edit
        </button>
        <button
            v-if="this.row.fieldName != 'id'"
            @click="$emit('delete', deleteData())"
            class="btn btn-danger"
        >
            Delete
        </button>
    </td>
    <td v-else>
        <button @click="$emit('save', saveData())" class="btn btn-primary">
            Save
        </button>
        <button @click="toggleEditing" class="btn btn-secondary">Cancel</button>
    </td>
    </tr>
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
            success: false,
        };
    },
    methods: {
        closeBtn() {
            this.errors = [];
            this.success = false;
        },
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
                this.errors = [];
                this.success = true;
                this.toggleEditing();
                return {
                    oldFieldName: this.row.fieldName,
                    oldFieldType: this.row.fieldType,
                    newFieldName: this.newFieldName,
                    newFieldType: this.newFieldType,
                };
            }
            this.success = false;
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
