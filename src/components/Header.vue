<template>
    <div>
  <b-nav tabs>
    <b-nav-item disabled><b> Quiz app </b></b-nav-item>
    <b-nav-item disabled>Counter: {{ numbCorrect }}/{{ numbTotal }}</b-nav-item>
  </b-nav>
</div>
</template>

<script>

export default { 
  props: [
    'numbCorrect',
    'numbTotal'
  ]
}
</script>