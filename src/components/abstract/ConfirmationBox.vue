<script setup lang="ts">
// TODO: Add CSS styling for box type (danger | warning | default).
const props = withDefaults(defineProps<{
  title?: string;
  content?: string;
  type?: 'danger' | 'warning' | 'default';
  closeIsCancel?: boolean;
  confirmLabel?: string;
  cancelLabel?: string;
}>(), {
  type: 'default',
  closeIsCancel: true,
  confirmLabel: 'Agree',
  cancelLabel: 'Cancel'
});

const emit = defineEmits<{
  confirm: [],
  cancel: [],
  close: [],
}>()

const close = () => {
  emit('close');
  if (props.closeIsCancel) {
    emit('cancel');
  }
}

const confirm = (closeModalFn: () => void) => {
  closeModalFn();
  emit('confirm');
}

const cancel = (closeModalFn: () => void) => {
  closeModalFn();
  emit('cancel');
}
</script>
<template>
  <Modal :title="title" @close="close">
    <template #input="{ open }">
      <slot name="input" :open="open" />
    </template>
    <template #content="{ close }">
      <div class="text-sm mb-5">
        <div class="space-y-2">
          <p v-if="content">{{ content }}</p>
        </div>
      </div>
      <!-- Modal footer -->
      <div class="flex flex-wrap justify-end space-x-2">
        <InputButton variant="secondary" @click="cancel(close)">
          {{ cancelLabel }}
        </InputButton>
        <InputButton  @click="confirm(close)">
          {{ confirmLabel }}
        </InputButton>
      </div>
    </template>
  </Modal>
</template>