<template>
  <div>
    <ul>
      <propertyControl
        thing-name="stage"
        property-name="position"
        label="position (mm)"
        :read-back="true"
        :read-back-delay="200"
      />
      <propertyControl
        thing-name="stage"
        property-name="velocity"
        label="velocity (mm/s)"
        :read-back="true"
        :read-back-delay="200"
      />
      <label class="uk-form-label">move rel delta:</label>
      <input v-model="delta" type="number" class="uk-form-small numeric-setting-line-input uk-form-label" />
      <actionButton
        thing="stage"
        action="move_rel"
        poll-interval="100"
        :submit-data="{ 'delta': delta }"
        submit-label="move rel"
      />
      <label class="uk-form-label">move abs target:</label>
      <input v-model="target" type="number" class="uk-form-small numeric-setting-line-input" />
      <actionButton
        thing="stage"
        action="move_abs"
        poll-interval="100"
        :submit-data="{ position: 12 }"
        submit-label="move abs"
      />
    </ul>
  </div>
</template>

<script>
import propertyControl from '@/components/labThingsComponents/propertyControl.vue'
import actionButton from '@/components/labThingsComponents/actionButton.vue'

export default {
  name: 'projectorControl',

  components: {
    propertyControl,
    actionButton,
  },

  data: function () {
    return {
      projectors: [
        { name: 'projector_r', label: 'Projector Red' },
        { name: 'projector_g', label: 'Projector Grn' },
        { name: 'projector_b', label: 'Projector Blu' },
      ],
      delta: 0.5,
      target: 0.0,
    }
  },
}
</script>

<style lang="less" scoped></style>
