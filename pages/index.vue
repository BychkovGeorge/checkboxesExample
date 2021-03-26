<template>
  <main class="mt-5 mw-100 container">
    <div class="row justify-content-center mb-5">
      <h3>Выбрать категорию</h3>
    </div>
    <div class="row justify-content-center">
      <Form v-on:save="showSelected($event)" :checkboxes="formData" />
    </div>
  </main>
</template>

<script>
import Form from '../components/form'

export default {
  components: { Form },
  data () {
    return {
      formData: [
        {
          header: 'Овощи',
          values: ['Огурцы', 'Морковь', 'Помидоры']
        },
        {
          header: 'Фрукты',
          values: ['Бананы', 'Яблоки', 'Груши']
        },
        {
          header: 'Ягоды',
          values: ['Малина', 'Клубника', 'Черника']
        }
      ]
    }
  },

  methods: {
    showSelected (e) {
      let message = 'Вы выбрали: '
      e.forEach((type) => {
        type.forEach((item) => {
          message += item + ' '
        })
      })
      alert(message)
    }
  }
}
</script>
