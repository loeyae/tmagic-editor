<template>
  <component
    class="tmagic-design-checkbox"
    :is="uiComponent"
    v-bind="uiProps"
    @update:modelValue="updateModelValue"
    @change="changeHandler"
  >
    <template #default v-if="$slots.default">
      <slot></slot>
    </template>
  </component>
</template>

<script setup lang="ts">
import { computed } from 'vue';

import { getDesignConfig } from './config';
import type { CheckboxProps } from './types';

defineOptions({
  name: 'TMCheckbox',
});

const props = withDefaults(defineProps<CheckboxProps>(), {
  trueValue: undefined,
  falseValue: undefined,
});

const ui = getDesignConfig('components')?.checkbox;

const uiComponent = ui?.component || 'el-checkbox';

const uiProps = computed<CheckboxProps>(() => ui?.props(props) || props);

const emit = defineEmits(['change', 'update:modelValue']);

const changeHandler = (v: any) => {
  emit('change', v);
};

const updateModelValue = (v: any) => {
  emit('update:modelValue', v);
};
</script>
