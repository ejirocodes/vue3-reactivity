<script lang="ts" setup>
import { computed, ref, type WritableComputedRef } from "vue";
import {
  Dialog,
  DialogOverlay,
  DialogTitle,
  TransitionChild,
  TransitionRoot,
} from "@headlessui/vue";
import { XCircleIcon } from "@heroicons/vue/24/outline";

const props = withDefaults(defineProps<Props>(), {
  isOpen: false,
  title: "This is the default title",
  body: "This is the default body",
});

const emit = defineEmits(["modalState"]);

const modalState = ref<boolean | null>(null);

interface Props {
  isOpen: boolean;
  title: string;
  body: string;
}

const isModalOpen: WritableComputedRef<boolean> = computed({
  get(): boolean {
    return props.isOpen;
  },
  set(newValue: boolean): boolean {
    modalState.value = newValue;
    return newValue;
  },
});

function closeModal() {
  isModalOpen.value = false;
  emit("modalState", modalState.value);
}
</script>

<template>
  <TransitionRoot as="template" :show="isOpen">
    <Dialog
      as="div"
      class="fixed z-10 inset-0 overflow-y-auto"
      @close="closeModal"
    >
      <div
        class="flex items-end justify-center min-h-screen pt-4 px-4 pb-20 text-center sm:block sm:p-0"
      >
        <TransitionChild
          as="template"
          enter="ease-out duration-300"
          enter-from="opacity-0"
          enter-to="opacity-100"
          leave="ease-in duration-200"
          leave-from="opacity-100"
          leave-to="opacity-0"
        >
          <DialogOverlay
            class="fixed inset-0 bg-gray-500 bg-opacity-75 transition-opacity"
          />
        </TransitionChild>

        <!-- This element is to trick the browser into centering the modal contents. -->
        <span
          class="hidden sm:inline-block sm:align-middle sm:h-screen"
          aria-hidden="true"
          >&#8203;</span
        >
        <TransitionChild
          as="template"
          enter="ease-out duration-300"
          enter-from="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
          enter-to="opacity-100 translate-y-0 sm:scale-100"
          leave="ease-in duration-200"
          leave-from="opacity-100 translate-y-0 sm:scale-100"
          leave-to="opacity-0 translate-y-4 sm:translate-y-0 sm:scale-95"
        >
          <div
            class="inline-block align-bottom bg-white rounded-lg px-[20px] pt-[18px] pb-[16px] text-left overflow-hidden shadow-xl transform transition-all sm:my-8 sm:align-middle sm:max-w-[510px] sm:w-full sm:p-6"
          >
            <div class="flex items-start justify-between">
              <slot name="icon"></slot>

              <div class="pl-[6px] pr-[28px] text-left">
                <DialogTitle
                  as="h3"
                  class="text-lg leading-6 font-medium text-gray-900"
                  >{{ title }}</DialogTitle
                >
                <div class="mt-2">
                  <p class="text-sm text-gray-500">{{ body }}</p>
                </div>
              </div>
              <button @click="closeModal">
                <XCircleIcon
                  class="h-[33px] w-[33px] p-[8px] text-gray-400 hover:bg-gray-100 rounded-full transition ease-in-out delay-150 hover:-translate-y-1 hover:scale-110 active:bg-gray-300 focus:bg-gray-300"
                  aria-hidden="true"
                />
              </button>
            </div>
          </div>
        </TransitionChild>
      </div>
    </Dialog>
  </TransitionRoot>
</template>
