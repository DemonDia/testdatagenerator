<template>
    <div id="generatorContainer">
        <div class="mb-3">
            <label class="form-label">File Name: </label>
            <input v-model="jsonFileName" class="form-control" />
        </div>
        <div class="mb-3">
            <label>Number of rows: </label>
            <input
                min="1"
                type="number"
                v-model="numberOfRows"
                class="form-control"
            />
            <br />
        </div>
        <button @click="generateJsonFile" class="btn" id = "generateBtn">
            Generate Json
        </button>
    </div>
</template>
<script>
export default {
    name: "GenerateJsonData",
    props: ["jsonData"],
    data() {
        return {
            numberOfRows: 1,
            jsonFileName: "test",
        };
    },
    methods: {
        gnerateDataColumn(entityName, entityType, id) {
            if (entityType == "string") {
                return entityName + " #" + id;
            }
            if (entityType == "boolean") {
                var outcome = [true, false];
                return outcome[Math.floor(Math.random() * 2)];
            }
            if (entityType == "integer") {
                return id;
            }
            if (entityType == "float") {
                var randomNumber = id + Math.random();
                return randomNumber.toFixed(2);
            }
        },
        generateJsonData() {
            var resultantJsonData = [];
            for (var i = 1; i <= this.numberOfRows; i++) {
                var jsonRow = {};
                for (var j = 0; j < this.jsonData.length; j++) {
                    var colName = this.jsonData[j].fieldName;
                    var colType = this.jsonData[j].fieldType;
                    jsonRow[colName] = this.gnerateDataColumn(
                        colName,
                        colType,
                        i
                    );
                }
                resultantJsonData.push(jsonRow);
            }
            return resultantJsonData;
        },
        generateJsonFile() {
            const data = JSON.stringify(this.generateJsonData());
            const blob = new Blob([data], { type: "text/plain" });
            const e = document.createEvent("MouseEvents"),
                a = document.createElement("a");
            a.download = this.jsonFileName + ".json";
            a.href = window.URL.createObjectURL(blob);
            a.dataset.downloadurl = ["text/json", a.download, a.href].join(":");
            e.initEvent(
                "click",
                true,
                false,
                window,
                0,
                0,
                0,
                0,
                0,
                false,
                false,
                false,
                false,
                0,
                null
            );
            a.dispatchEvent(e);
        },
    },
};
</script>
<style scoped>
#generatorContainer {
    width: 70%;
    display: inline-block;
    padding:10px;
    background:rgb(50, 50, 50);
    border-radius: 5px;
}
#generatorContainer label {
    float: left;
}
#generateBtn{
    background: black;
    color: white;
}
</style>
