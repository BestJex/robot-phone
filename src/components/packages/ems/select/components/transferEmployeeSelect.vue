<template>
  <span>
    <Select v-model="computedValue"
            v-bind="options"
            :placeholder="placeholder"
            :clearable="clearable"
            :filterable="filterable"
            @on-change="change"
            v-show="list.length || !showNodata">
      <Option v-for="v in list"
              :value="v.id"
              :key="v.id">{{ v.label }}</Option>
    </Select>
    <p class="nodata"
       v-show="list.length === 0 && showNodata">{{nodataText}}</p>
  </span>
</template>

<script>
import selectMixin from './selectMixin'
import taskApi from '@/api/ems/task'

export default {
  mixins: [selectMixin],
  props: {
    params: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  methods: {
    getData() {
      return taskApi.listTransferEmployee(this.params).then(data => {
        return data.map(v => {
          return {
            id: v.id,
            label: v.employeeName,
            data
          }
        })
      })
    }
  }
}
</script>
