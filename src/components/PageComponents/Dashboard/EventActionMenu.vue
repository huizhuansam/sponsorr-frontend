<template>
  <v-menu>
    <template #activator="{on, attrs}">
      <v-btn icon v-bind="attrs" v-on="on">
        <v-icon>mdi-dots-vertical</v-icon>
      </v-btn>
    </template>

    <v-list>
      <v-list-item v-if="eventCategory.name === EventGroup.Draft">
        <EventPublish @publishEvent="$emit('publishEvent', event)" />
      </v-list-item>
      <v-list-item v-if="eventCategory.name === EventGroup.Published">
        <EventUnpublish @publishEvent="$emit('unpublishEvent', event)" />
      </v-list-item>
      <v-list-item v-if="eventCategory.name !== EventGroup.Matched">
        <EventDelete
          :event-id="event.eventId"
          :title="event.title"
          :status="event.status"
          @deleteEvent="(payload) => $emit('deleteEvent', payload)"
        />
      </v-list-item>
      <v-list-item>
        <EventView :event-id="event.eventId" />
      </v-list-item>
    </v-list>
  </v-menu>
</template>

<script lang="ts">
import EventDelete from '@/components/EventActions/EventDelete.vue';
import EventPublish from '@/components/EventActions/EventPublish.vue';
import EventUnpublish from '@/components/EventActions/EventUnpublish.vue';
import EventView from '@/components/EventActions/EventView.vue';
import { EventCategory, SponsorEvent } from '@/types';
import { EventGroup } from '@/types/enum';
import { defineComponent } from '@vue/composition-api';

export default defineComponent({
  components: {
    EventPublish,
    EventUnpublish,
    EventDelete,
    EventView,
  },
  props: {
    event: {
      type: Object as () => SponsorEvent,
      required: true,
    },
    eventCategory: {
      type: Object as () => EventCategory,
      required: true,
    },
  },
  setup() {
    return { EventGroup };
  },
});
</script>
