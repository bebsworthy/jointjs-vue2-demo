<template>
    <JointGraph />
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import JointGraph from '@/components/JointGraph.vue'; // @ is an alias to /src

@Component({
  components: {
    JointGraph,
  },
})
export default class Home extends Vue {}
</script>
