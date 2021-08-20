<template>
  <div :class="['modal', { show: isValidForm }]">Клиент успешно создан!</div>
</template>

<script>
export default {
  props: ["isValidForm"],
};
</script>