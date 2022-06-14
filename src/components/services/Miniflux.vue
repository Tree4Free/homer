<template>
  <Generic :item="item">
    <template #indicator>
      <div class="notifs">
        <strong v-if="unread > 0" class="notif unread" title="Unread">
          {{ unread }}
        </strong>
      </div>
    </template>
  </Generic>
</template>

<script>
import service from "@/mixins/service.js";
import Generic from "./Generic.vue";

export default {
  name: "Miniflux",
  mixins: [service],
  props: {
    item: Object,
  },
  components: {
    Generic,
  },
  data: () => ({
    unread: null,
  }),
  created() {
    this.fetchAllUnread();
  },
  methods: {
    fetchAllUnread: async function () {
      const headers = {
        "X-Auth-Token": this.item.apikey,
      };
      const unread = await this.fetch("/v1/entries?status=unread", { headers })
        .catch((e) => console.error(e));

      this.unread = unread.total;
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

    &.unread {
      background-color: #4fd671;
    }
  }
}
</style>
