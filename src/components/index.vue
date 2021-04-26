<template>
    <v-container>
        <v-row class="text-center">
            <v-col cols="12">
                <v-btn @click="readDirectory">
                    <span class="mr-2">Browse Folder</span>
                </v-btn>
                <h1>{{ chosenDir }} </h1>
                <v-btn color="secondary" x-large @click="getFiles">
                    <span class="mr-2">List Files</span>
                </v-btn>
                <ul>
                    <li v-for="file in listOfFiles" :key="file">
                        {{ file }}
                    </li>
                </ul>
            </v-col>
        </v-row>
    </v-container>
</template>
<script>
const { remote } = require('electron');
const fs = require('fs');
export default {
    data: () => ({
        chosenDir: '',
        listOfFiles: [],
    }),
    methods: {
        readDirectory() {
            console.log("starting")
            remote.dialog.showOpenDialog({
            properties: ['openFile', 'openDirectory']
            }).then(result => {
                this.chosenDir = result.filePaths[0]
                // this.getFiles()
            // console.log(result.canceled)
            console.log(result.filePaths)
            }).catch(err => {
            console.log(err)
            });
        },
        getFiles() {
            fs.readdir(this.chosenDir, (err, dir) => {
            this.listOfFiles = dir;
            // for(let filePath of dir)
            //     console.log(filePath);
            });
        }
    }
}
</script>
