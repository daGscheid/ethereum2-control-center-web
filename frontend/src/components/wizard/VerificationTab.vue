<template>
  <div>
    <span v-if="!running">
      Please check the entered information and start the installation
    </span>
    <span v-if="running">
      <b>Installation in Progress, please be patient</b>
    </span>
    <data>
      <table class="text-left table">
        <tr>
          <td class="table-label">Network:</td>
          <td class="table-data">{{ model.network }}</td>
        </tr>
        <tr>
          <td class="table-label">Setup:</td>
          <td class="table-data">{{ model.client }}</td>
        </tr>
        <tr>
          <td class="table-label">Customization:</td>
          <td class="table-data">{{ model.override }}</td>
        </tr>
        <tr>
          <td class="table-label">Ethereum 1 nodes:</td>
          <td class="table-data">
            <b-table striped hover :items="model.eth1nodes"> </b-table>
          </td>
        </tr>
        <tr>
          <td class="table-label">Updates:</td>
          <td class="table-data">
            <div
              v-if="
                !(typeof model.updates.unattended === 'undefined') &&
                model.updates.unattended.indexOf('check') > -1
              "
            >
              Check for updates automatically: <strong>Yes</strong>
              <br />Unattended update installation:
              <strong v-if="model.updates.unattended.indexOf('install') > -1"
                >Yes</strong
              ><strong v-else>No</strong> <br />Lane:
              <strong>{{ model.updates.lane }}</strong>
            </div>
            <div v-else>Check for updates: <strong>No</strong></div>
          </td>
        </tr>
        <tr>
          <td class="table-label">Installation Folder:</td>
          <td class="table-data">{{ model.installationFolder }}</td>
        </tr>
      </table>

      <div v-if="running || done">
        <b-progress
          v-if="running"
          :value="progress"
          variant="info"
          :max="max"
          show-progress
          animated
        >
          <b-progress-bar :value="progress">
            <span
              >Progress: <strong>{{ progress.toFixed(0) }}%</strong></span
            >
          </b-progress-bar>
        </b-progress>
        <strong class="text-left">Logs:</strong>
        <ul class="list-group list-unstyled">
          <task-status-entry
            class="list-group-item text-left"
            v-bind:key="index"
            v-for="(status, index) in logs"
            :model="status"
          ></task-status-entry>
        </ul>
      </div>
    </data>
  </div>
</template>

<script>
import TaskStatusEntry from "@/components/commons/TaskStatusEntry.vue";

export default {
  name: "VerificationTab",
  components: {
    TaskStatusEntry,
  },
  props: {
    model: Object,
    running: Boolean,
    done: Boolean,
    progress: Number,
    logs: Array,
  },
  data() {
    return {
      max: 100,
    };
  },
  methods: {},
};
</script>

<style scoped></style>
