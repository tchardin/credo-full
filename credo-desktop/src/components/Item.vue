<template>
  <tr>
    <th>{{model.name}}</th>
    <td :class="[model.verified ? 'verified' : 'unverified']">{{model.verified ? 'Verified' : 'Unverified'}}</td>
  </tr>
</template>

<script>
export default {
  name: 'item',
  props: {
    model: Object
  }
}
</script>

<style lang="scss" scoped>
.verified {
  color: #00C96D;
}
.unverified {
  color: #FF5964;
}
</style>
