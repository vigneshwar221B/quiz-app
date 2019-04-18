<template>
<div>
  <b-nav tabs>
    <b-nav-item disabled><b>Quiz app</b></b-nav-item>
    <b-nav-item disabled>Score : {{ score }}/10</b-nav-item>
    </b-nav>
</div>
</template>

<script>
export default {
  props : [
    'score'
  ]
}
</script>