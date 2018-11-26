<template>
  <section>
    <div id="person" class="list">
       {{person in persons}}
    </div>
  </section>
</template>

<script>
import axios from 'axios';

export default({
  data() {
    return {
      persons: ['']
    }
  },
  methods: {
      async asyncData(context) {
        let personData = await axios({
                        method: 'get',
                        url: 'http://localhost:4567/person-database/person/list'
                      })
                      return { persons : personData.data }
  }
}
})
</script>
