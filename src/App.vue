<template>
  <div id='q-app'>
    <router-view />
  </div>
</template>

<script>
export default {
  name: 'App',
  prod: 'localhost:8081/#/',
  // prod: 'localhost:8080/recruitment-task/#/',
  host: 'localhost:8080/'
  // host: 'localhost:8080/recruitment-task-1.0'
}
</script>
