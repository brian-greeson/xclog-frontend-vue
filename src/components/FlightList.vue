<template>
  <div class="item">
    <table class="ui celled striped table">
      <thead>
        <tr>
          <th colspan="3">
            <h3>Flight List</h3>
            <div v-if="selectedFiles" class="ui indicating progress">
              <div class="bar">
                <div class="progress"></div>
              </div>
              <div class="label">{{ message }}</div>
            </div>
            <div v-else>
              <label class="ui left icon input">
                <input type="file" ref="file" @change="selectFile" />
                <i class="file icon"></i>
              </label>
            </div>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td class="collapsing">12/11/2019</td>
          <td>Boulder</td>
          <td class="right aligned collapsing">1:09</td>
        </tr>
        <tr>
          <td>06/15/2020</td>
          <td>Mt. Herman</td>
          <td class="right aligned">4:31</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import UploadService from "../services/TrackUpload";

export default {
  name: "FlightList",
  data() {
    return {
      selectedFiles: undefined,
      currentFile: undefined,
      progress: 0,
      message: "Uploading",
      fileInfos: [],
    };
  },
  methods: {
    selectFile() {
      this.selectedFiles = this.$refs.file.files;
    },
    upload() {
      this.progress = 0;

      this.currentFile = this.selectedFiles.item(0);
      UploadService.upload(this.currentFile, (event) => {
        this.progress = Math.round((100 * event.loaded) / event.total);
      })
        .then((response) => {
          this.message = response.data.message;
          return UploadService.getFiles();
        })
        .then((files) => {
          this.fileInfos = files.data;
        })
        .catch(() => {
          this.progress = 0;
          this.message = "Could not upload file";
          this.currentFile = undefined;
        });

      this.selectedFiles = undefined;
    },
  },
  mounted() {
    UploadService.getFiles().then((response) => {
      this.fileInfos = response.data;
    });
  },
};
</script>

<style>
</style>