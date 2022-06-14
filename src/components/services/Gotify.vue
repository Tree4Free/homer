<template>
  <Generic :item="item">
    <template #indicator>
      <div class="notifs">
        <strong v-if="msgCount > 0" class="notif messages" title="Messages">
          {{ msgCount }}
        </strong>
      </div>
    </template>
  </Generic>
</template>

<script>
import service from "@/mixins/service.js";
import Generic from "./Generic.vue";

export default {
  name: "Gotify",
  mixins: [service],
  props: {
    item: Object,
  },
  components: {
    Generic,
  },
  data: () => ({
    msgCount: null,
  }),
  created() {
    this.fetchMsgCount();
  },
  methods: {
    fetchMsgCount: async function () {
      const headers = {
        "X-Gotify-Key": this.item.apikey,
      };
      const messages = await this.fetch("/message", { headers })
        .catch((e) => console.error(e));

      this.msgCount = messages.paging.size;
    },
  },
};
</script>

<style scoped lang="scss">
.notifs {
  position: absolute;
  color: white;
  font-family: sans-serif;
  top: 0.3em;
  right: 0.5em;

  .notif {
    display: inline-block;
    padding: 0.2em 0.35em;
    border-radius: 0.25em;
    position: relative;
    margin-left: 0.3em;
    font-size: 0.8em;

    &.messages {
      background-color: #4fd671;
    }
  }
}
</style>
