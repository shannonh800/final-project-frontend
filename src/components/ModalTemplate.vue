<template>
    <Transition name="fade">
        <div v-if="showing" class="fixed inset-0 w-full h-screen flex items-center justify-center bg-bg-semi-75" @click.self="close">
            <slot/>
        </div>
    </Transition>
</template>
   
<script>
export default {
  name: "ModalTemplate",
  props: {
    showing: {
      required: true
    }
  },
  methods: {
      close() {
          this.$emit("close");
      }
  }
}
</script>

<style scoped>
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>