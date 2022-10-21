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
        </div>
        <div class="mb-3">
            <label>File type:</label>
            <select v-model="generatedFileType" class="form-control">
                <option v-for="(value, key) in fileTypes" v-bind:key="key">
                    {{ value }}
                </option>
            </select>
            <br />
        </div>

        <button @click="generateDataFile" class="btn" id="generateBtn">
            Generate Test Data
        </button>
    </div>
</template>
<script>
export default {
    name: "GenerateData",
    props: ["jsonData"],
    data() {
        return {
            numberOfRows: 1,
            jsonFileName: "test",
            generatedFileType: "json",
            fileTypes: ["json", "csv"],
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
        // ======================generate the data file======================
        generateDataFile() {
            if (this.generatedFileType == "json") {
                this.generateJsonFile();
            } else {
                this.generateCsvFile();
            }
        },
        // ====================for json====================
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
        // ====================for csv====================
        generateCsvData() {
            var csvData = "";
            var columns = [];
            // get the columns
            for (var h = 0; h < this.jsonData.length; h++) {
                var colTitle = this.jsonData[h].fieldName;
                columns.push(colTitle);
            }
            csvData += columns.join(",");
            csvData += "\n";

            // each row
            for (var i = 1; i <= this.numberOfRows; i++) {
                var newRow = [];
                for (var j = 0; j < this.jsonData.length; j++) {
                    var colName = this.jsonData[j].fieldName;
                    var colType = this.jsonData[j].fieldType;
                    newRow.push(this.gnerateDataColumn(colName, colType, i));
                }
                csvData += newRow.join(",");
                csvData += "\n";
            }
            return csvData;
        },

        generateCsvFile() {
            var csvData = this.generateCsvData();
            // and passing the data with type
            const blob = new Blob([csvData], { type: "text/csv" });

            // Creating an object for downloading url
            const url = window.URL.createObjectURL(blob);

            // Creating an anchor(a) tag of HTML
            const a = document.createElement("a");

            // Passing the blob downloading url
            a.setAttribute("href", url);
            // Setting the anchor tag attribute for downloading
            // and passing the download file name
            a.setAttribute("download", this.jsonFileName+".csv");
            // Performing a download with click
            a.click();
        },
    },
};
</script>
<style scoped>
#generatorContainer {
    width: 70%;
    display: inline-block;
    padding: 10px;
    background: rgb(50, 50, 50);
    border-radius: 5px;
}
#generatorContainer label {
    float: left;
}
#generateBtn {
    background: black;
    color: white;
}
</style>
