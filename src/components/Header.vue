<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled><b>CSE Quiz App</b></b-nav-item>
      <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }} </b-nav-item>
      <b-nav-item disabled>You have total {{ total }} questions</b-nav-item>
    </b-nav>
  </div>
</template>

<script>
export default {
    props: [
        'numCorrect',
        'numTotal',
        'total'
    ]
}
</script>