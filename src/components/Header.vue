<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        Fancy Quiz App
      </b-nav-item>
      <b-nav-item disabled>
        Counter: {{ numCorrect }}/{{ numTotal }}
      </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    numCorrect: {
      type: Number,
      default: 0
    },
    numTotal: {
      type: Number,
      default: 0
    }
  }
}
</script>
