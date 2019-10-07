<template>
  <div>
    <input type="text" v-model="content" />
    <p>{{ zhouyun }}</p>
  </div>
</template>

<script>
export default {
  name: "inputBtn",
  computed: {
    zhouyun: vm => {
      let amount = vm.content ? Number(vm.content) : "";
      return amount.toLocaleString("en-IN", {
        style: "currency",
        currency: "INR",
        minimumFractionDigits: 2
      });
    }
  },
  data() {
    return {
      content: ""
    };
  },
  methods: {}
};
</script>

<style scoped></style>
