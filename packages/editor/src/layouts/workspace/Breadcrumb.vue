<template>
  <div v-if="nodes.length === 1" class="m-editor-breadcrumb">
    <template v-for="(item, index) in path" :key="item.id">
      <TMagicButton link :disabled="item.id === node?.id" @click="select(item)">{{ item.name }}</TMagicButton
      ><span v-if="index < path.length - 1">/</span>
    </template>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

import type { MNode } from '@tmagic/core';
import { TMagicButton } from '@tmagic/design';
import { getNodePath } from '@tmagic/utils';

import { useServices } from '@editor/hooks/use-services';

defineOptions({
  name: 'MEditorBreadcrumb',
});

const { editorService } = useServices();

const node = computed(() => editorService.get('node'));
const nodes = computed(() => editorService.get('nodes'));
const root = computed(() => editorService.get('root'));
const path = computed(() => getNodePath(node.value?.id || '', root.value?.items || []));

const select = async (node: MNode) => {
  await editorService.select(node);
  editorService.get('stage')?.select(node.id);
};
</script>
