<template>
  <draggable
    :list="sensors"
    group="sensors"
    tag="ul"
    class="list-group dragArea"
    :class="{ 'list-group-flush': flush }"
  >
    <li v-for="sensor in sensors" :key="sensor.identifier" class="list-group-item-container">
      <sensor-registry-entry :sensor="sensor" @remove="remove(sensor)" class="list-group-item" />
      <div v-if="isAggregatedSensor(sensor)">
        <dragable-sensor-list :sensors="sensor.children" />
      </div>
    </li>
  </draggable>
</template>

<script lang="ts">
import { Vue, Component, Prop } from 'vue-property-decorator'
import draggable from 'vuedraggable'

import { Sensor, AggregatedSensor } from '@/model/SensorRegistry'

import SensorRegistryEntry from '@/components/SensorRegistryEntry.vue'

@Component({
  components: {
    draggable,
    SensorRegistryEntry
  },
  name: 'DragableSensorList'
})
export default class DragableSensorList extends Vue {
  @Prop({ required: true }) sensors!: Sensor[];

  @Prop({ type: Boolean }) flush = false;

  private isAggregatedSensor = (sensor: Sensor) =>
    sensor instanceof AggregatedSensor;

  private remove (sensor: Sensor) {
    const index = this.sensors.indexOf(sensor, 0)
    if (index > -1) {
      this.sensors.splice(index, 1)
    }
  }
}
</script>

<style scoped>
.list-group-item-container {
  display: block;
}
.list-group-item {
  margin-bottom: -1px;
}
.list-group-flush .list-group-item {
  border-right: 0;
  border-left: 0;
}
.card-header+.list-group-flush > .list-group-item-container:first-child > .list-group-item {
  border-top: 0;
}
.list-group .list-group-item:first-child,
.list-group
  .list-group
  .list-group-item-container:first-child
  .list-group-item {
  border-top-left-radius: 0;
  border-top-right-radius: 0;
}
.list-group .list-group-item:last-child,
.list-group .list-group .list-group-item-container:last-child .list-group-item {
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
}
.list-group .list-group .list-group-item {
  padding-left: 2.5rem;
}
.list-group .list-group .list-group .list-group-item {
  padding-left: 3.75rem;
}
.list-group .list-group .list-group .list-group .list-group-item {
  padding-left: 5rem;
}
.list-group .list-group .list-group .list-group .list-group .list-group-item {
  padding-left: 6.25rem;
}

.list-group:empty {
  min-height: 1em;
  background-color: white;
  border: 1px solid rgba(0, 0, 0, 0.125);
  border-top: 0 none;
  border-bottom-right-radius: 0;
  border-bottom-left-radius: 0;
  border-top-right-radius: 0;
  border-top-left-radius: 0;
  margin-bottom: -1px;
}
.list-group-flush.list-group:empty  {
  border-right: 0;
  border-left: 0;
}
.list-group .list-group:empty {
  padding-left: 2.5rem;
}
.list-group .list-group .list-group:empty {
  padding-left: 3.75rem;
}
.list-group .list-group .list-group .list-group:empty {
  padding-left: 5rem;
}
.list-group .list-group .list-group .list-group .list-group:empty {
  padding-left: 6.25rem;
}
.list-group:empty::before {
  background-color: rgba(0, 0, 0, 0.05);
  content: "no sensors";
  text-align: center;
  color: rgba(0, 0, 0, 0.5);
  font-size: 1em;
  line-height: 2em;
}
.list-group-item-container .list-group:empty::before {
  content: "no child sensors";
}
.list-group-flush .list-group:empty {
  border-right: 0;
  border-left: 0;
}

.sortable-ghost {
  border: 2px dashed steelblue;
}
</style>
