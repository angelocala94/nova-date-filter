<template>
  <div>
    <filter-select
      v-for="filter in filters"
      :key="filter.name"
    >
      <h3
        slot="default"
        class="text-sm uppercase tracking-wide text-80 bg-30 p-3"
      >
        {{ filter.name }}
      </h3>
      <DatePicker
        v-if="filter.isDateFilter"
        v-model="filter.currentValue"
        :options="filter.options"
        @input="filterChanged(filter)"
      />
      <select
        v-else
        slot="select"
        :dusk="filter.name + '-filter-select'"
        class="block w-full form-control-sm form-select"
        v-model="filter.currentValue"
        @change="filterChanged(filter)"
      >
        <option
          value=""
          selected
        >&mdash;</option>

        <option
          v-for="option in filter.options"
          :key="option.value"
          :value="option.value"
        >
          {{ option.name }}
        </option>
      </select>
    </filter-select>
  </div>
</template>

<script>
import DatePicker from './DatePicker';

export default {
  components: { DatePicker },

  props: ['filters', 'currentFilters'],

  /**
   * Mount the component.
   */
  mounted() {
    this.current = this.currentFilters;
  },

  methods: {
    /**
     * Handle a filter selection change.
     */
    filterChanged(filter) {
      this.current = _.reject(this.current, f => f.class == filter.class);

      if (filter.currentValue !== '') {
        this.current.push({
          class: filter.class,
          value: filter.currentValue
        });
      }

      this.$emit('update:currentFilters', this.current);
      this.$emit('changed');
    }
  }
};
</script>
