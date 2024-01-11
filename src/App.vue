<template>
  <div class="wrapper">
    <div class="container">
      <div class="content">
        <div class="server-list">
          <div class="card">
            <ul>
              <li
                v-for="(server, index) in servers"
                :key="index"
                @click="selectServer(server)"
              >
                {{ server.server_name }}
              </li>
            </ul>
          </div>
        </div>

        <div class="server-form" v-if="selectedServer">
          <input
            v-model="selectedServer.server_name"
            type="text"
            autocomplete="off"
            name="text"
            class="input"
            placeholder="Server Name"
          />
          <select
            v-model="selectedServer.server_type"
            class="select"
            name="serverType"
          >
            <option value="vds">VDS</option>
            <option value="dedicated">Dedicated</option>
            <option value="hosting">Hosting</option>
          </select>
          <button @click="saveData" class="save-button">Save</button>
          <p v-if="saveMessage" class="save-message">{{ saveMessage }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted, watch } from "vue";

export default {
  setup() {
    const servers = ref(
      JSON.parse(localStorage.getItem("servers")) || [
        {
          customer_id: "user1",
          server_name: "server7",
          server_type: "vds",
        },
        {
          customer_id: "user5",
          server_name: "server2",
          server_type: "dedicated",
        },
        {
          customer_id: "user3",
          server_name: "server4",
          server_type: "hosting",
        },
      ]
    );

    const selectedServer = ref(
      JSON.parse(localStorage.getItem("selectedServer")) || null
    );

    const selectServer = (server) => {
      selectedServer.value = server;
      localStorage.setItem("selectedServer", JSON.stringify(server));
    };

    const saveMessage = ref("");

    const saveData = () => {
      localStorage.setItem("servers", JSON.stringify(servers.value));
      saveMessage.value = "Data saved successfully";
      setTimeout(() => {
        saveMessage.value = "";
      }, 3000);
    };

    watch(
      servers,
      (newServers) => {
        localStorage.setItem("servers", JSON.stringify(newServers));
      },
      { deep: true }
    );

    return { servers, selectedServer, selectServer, saveData, saveMessage };
  },
};
</script>

<style lang="scss">
</style>
