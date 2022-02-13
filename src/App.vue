<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
        <v-icon large>mdi-cloud-upload-outline</v-icon>
        <h1>CSV Uploader</h1>
      </div>
    </v-app-bar>

    <v-main>
      <v-container>
        <v-row>
          <v-col>
            <v-alert v-if="alertMessage" :type="alertType">{{ alertMessage }}</v-alert>
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <h2>Upload a New File</h2>
            <form>
              <v-file-input
                v-model="fileInput"
                counter
                show-size
                required
                truncate-length="0"
                hide-details="auto"
                autocomplete="off"
                :multiple="false"
                :accept="acceptedFileFormats.toString()"
              ></v-file-input>
              <v-btn
                class="mr-4"
                :disabled="!canSubmit"
                @click="submit"
              >
                submit
              </v-btn>
              <v-btn @click="clear">
                clear
              </v-btn>
            </form>
          </v-col>
          <v-col cols="6">
            <h2>Load an Existing File</h2>
            <v-select :items="files" v-model="fileSelected" @change="getFile" />
          </v-col>
        </v-row>
        <v-row>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: 'App',

  data: () => ({
    fileInput: null,
    fileSelected: null,
    files: null,
    acceptedFileFormats: [
      '.csv',
    ],
    alertMessage: null,
    alertType: null,
  }),

  created() {
    this.getFiles();
  },

  computed: {
    canSubmit() {
      return !! this.fileInput;
    },
  },

  methods: {
    async getFile () {
      const getFile = `${process.env.VUE_APP_API_URL}/file?key=${this.fileSelected}`;
      try {
        const { data } = await axios.get(getFile);
        console.log(data);
      } catch (error) {
        this.alertMessage = `Could not load selected file: ${this.fileSelected}. ${error}`;
        this.alertType = 'error';
      }
    },
    async getFiles () {
      const getFilesEndpoint = `${process.env.VUE_APP_API_URL}/files`;
      try {
        const { data } = await axios.get(getFilesEndpoint);
        this.files = data.map(file => `${file.Key}`);
      } catch (error) {
        this.alertMessage = `Could not get file list from ${getFilesEndpoint}. ${error}`;
        this.alertType = 'error';
      }
    },
    async submit () {
      let formData = new FormData();
      formData.append("input_file", this.fileInput, this.fileInput.name);
      try {
        const { data } = await axios.post(`${process.env.VUE_APP_API_URL}/files`, formData);
        this.alertMessage = data.message;
        this.alertType = 'success';
      } catch (error) {
        this.alertMessage = error;
        this.alertType = 'error';
      }
    },
    clear () {
      this.fileInput = null;
      this.alertMessage = null;
      this.alertType = null;
    },
  },
};
</script>
