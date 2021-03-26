<template>
  <main id="main" class="container mw-100">
    <div class="row">
      <div v-for="(checkbox, index) in checkboxes" :key="checkbox.header" class="col">
        <Checkbox v-on:knock="handler($event, index)" :values="checkbox.values" :header="checkbox.header" />
      </div>
    </div>
    <div class="row justify-content-end mr-5">
      <button @click="submit" class="btn btn-primary">Сохранить</button>
    </div>
  </main>
</template>

<script>
import Checkbox from './checkbox'
export default {
  components: { Checkbox },

  data () {
    return {
      formData: []
    }
  },

  props: {
    checkboxes: Array
  },

  mounted () {
    this.checkboxes.forEach((el) => {
      this.formData.push([])
    })
  },

  methods: {
    handler (e, index) {
      this.formData[index] = []
      e.forEach((el, num) => {
        if (el === true) {
          this.formData[index].push(this.checkboxes[index].values[num])
        }
      })
    },

    submit () {
      this.$emit('save', this.formData)
    }
  }
}
</script>
