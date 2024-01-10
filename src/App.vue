<template>
  <div class="wrapper">
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
      <select v-model="selectedServer.server_type" class="select" name="serverType">
        <option value="vds">VDS</option>
        <option value="dedicated">Dedicated</option>
        <option value="hosting">Hosting</option>
      </select>
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

    watch(servers, (newServers) => {
      localStorage.setItem("servers", JSON.stringify(newServers));
    }, { deep: true });

    return { servers, selectedServer, selectServer };
  },
};
</script>



<style lang="scss">
.wrapper {
  display: flex;
  gap: 50px;
  justify-content: center;
  align-items: baseline;
  margin-top: 200px;
}

.server-list {
  ul {
    list-style: none;
    padding: 0;
  }
  li {
    cursor: pointer;
    margin-bottom: 10px;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
    width: 160px;
  }
}

.server-form {
  margin-left: 20px;
}
</style>
