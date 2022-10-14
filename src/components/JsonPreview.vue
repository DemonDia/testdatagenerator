<template>
    <div>
        <h4>Preview</h4>
        <div></div>
        <code>
            <pre>{{ formatJsonFields }}</pre>
        </code>
    </div>
</template>
<script>
export default {
    name: "JsonPreview",
    props: ["jsonFields"],
    data() {},
    computed: {
        formatJsonFields() {
            var newJsonObject = {};
            this.jsonFields.fields.forEach((jsonItem) => {
                newJsonObject[jsonItem.fieldName] = jsonItem.fieldType;
            });
            console.log(JSON.stringify(newJsonObject, null, 2));
            return JSON.stringify(newJsonObject, null, 1);
        },
    },
};
</script>
<style scoped>
pre {
    text-align: left;
    background: black;
    padding: 20px;
    border-radius: 5px;
}
</style>
