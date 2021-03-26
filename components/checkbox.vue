<template>
  <main class="row justify-content-center">
    <ul style="list-style: none">
      <li @click="headerSelect" class="row pointer align-items-center mb-2">
        <div v-if="headerState.notChecked" class="custom-checkbox" />
        <div v-if="headerState.semiChecked" class="custom-checkbox-selected position-relative">
          <p class="center" style="color: white">
            -
          </p>
        </div>
        <div v-if="headerState.checked" class="custom-checkbox-selected position-relative">
          <p class="center" style="color: white">
            ✓
          </p>
        </div>
        <p class="my-auto ml-2">
          {{ header }}
        </p>
        <p class="my-auto ml-2">
          (выбрано {{ current }} из {{ values.length }})
        </p>
      </li>
      <ul style="list-style: none">
        <li v-for="(el, index) in values" :key="el" class="row pointer align-items-center" @click="select(index)">
          <div v-if="!simpleStates[index]" class="custom-checkbox" />
          <div v-else class="custom-checkbox-selected position-relative">
            <p class="center" style="color: white">
              ✓
            </p>
          </div>
          <p class="my-auto ml-2">
            {{ el }}
          </p>
        </li>
      </ul>
    </ul>
  </main>
</template>

<script>
export default {
  props: {
    header: String,
    values: Array
  },

  data () {
    return {
      simpleStates: [],
      current: 0,
      headerState: {
        checked: false,
        semiChecked: false,
        notChecked: true
      }
    }
  },

  mounted () {
    this.values.forEach((el) => {
      this.simpleStates.push(false)
    })
  },

  methods: {
    select (index) {
      this.simpleStates[index] = !this.simpleStates[index]
      let counter = 0
      this.simpleStates.forEach((el) => {
        if (el === true) {
          counter += 1
        }
      })
      switch (counter) {
        case 0:
          this.headerState.notChecked = true
          this.headerState.semiChecked = false
          this.headerState.checked = false
          break
        case this.values.length:
          this.headerState.notChecked = false
          this.headerState.semiChecked = false
          this.headerState.checked = true
          break
        default:
          this.headerState.notChecked = false
          this.headerState.semiChecked = true
          this.headerState.checked = false
          break
      }
      this.count()
      this.$emit('knock', this.simpleStates)
      this.$forceUpdate()
    },

    headerSelect () {
      if (this.headerState.semiChecked || this.headerState.notChecked) {
        this.headerState.checked = true
        this.headerState.semiChecked = false
        this.headerState.notChecked = false
        this.simpleStates = this.simpleStates.map(el => true)
      } else if (this.headerState.checked) {
        this.headerState.checked = false
        this.headerState.semiChecked = false
        this.headerState.notChecked = true
        this.simpleStates = this.simpleStates.map(el => false)
      }
      this.count()
      this.$emit('knock', this.simpleStates)
      this.$forceUpdate()
    },

    count () {
      this.current = 0
      this.simpleStates.forEach((el) => {
        if (el === true) {
          this.current += 1
        }
      })
    }
  }
}
</script>

<style>
.custom-checkbox {
  width: 16px;
  height: 16px;
  border: 1.5px solid grey;
  box-sizing: border-box;
  border-radius: 2px;
}

.custom-checkbox-selected {
  width: 16px;
  height: 16px;
  border: 1.5px solid grey;
  box-sizing: border-box;
  border-radius: 2px;
  background: #0064E7;
}

.center {
  position: absolute;
  top: 40%;
  left: 50%;
  margin-right: -50%;
  transform: translate(-50%, -50%)
}

.pointer {
  cursor: pointer;
}
</style>
