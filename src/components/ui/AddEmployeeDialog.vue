<template>
<v-dialog v-model="addEmployeeDialogVisibility">
</v-dialog>
</template>

<script>
export default {
  name: 'AddEmployeeDialog',
  data: () => ({
    id: null,
    firstName: null,
    lastName: null,
    email: null,
    phoneNumber: null,
    address: null
  }),
  props: {
    addEmployeeDialogVisibility: Boolean
  }
}
</script>

<style scoped>

</style>
