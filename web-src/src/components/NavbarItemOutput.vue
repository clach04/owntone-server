<template>
  <div class="navbar-item">
    <div class="level is-mobile">
      <div class="level-left fd-expanded">
        <div class="level-item" style="flex-grow: 0">
          <a class="button is-white is-small">
            <span
              class="icon fd-has-action"
              :class="{ 'has-text-grey-light': !output.selected }"
              @click="set_enabled"
              ><mdicon :name="type_class" size="18" :title="output.type"
            /></span>
          </a>
        </div>
        <div class="level-item fd-expanded">
          <div class="fd-expanded">
            <p
              class="heading"
              :class="{ 'has-text-grey-light': !output.selected }"
              v-text="output.name"
            />
            <Slider
              v-model="volume"
              :min="0"
              :max="100"
              :step="1"
              :tooltips="false"
              :disabled="!output.selected"
              :classes="{ target: 'slider' }"
              @change="set_volume"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Slider from '@vueform/slider'
import webapi from '@/webapi'

export default {
  name: 'NavbarItemOutput',
  components: {
    Slider
  },

  props: ['output'],

  computed: {
    type_class() {
      if (this.output.type.startsWith('AirPlay')) {
        return 'cast-variant'
      } else if (this.output.type === 'Chromecast') {
        return 'cast'
      } else if (this.output.type === 'fifo') {
        return 'pipe'
      } else {
        return 'server'
      }
    },

    volume() {
      return this.output.selected ? this.output.volume : 0
    }
  },

  methods: {
    play_next: function () {
      webapi.player_next()
    },

    set_volume: function (newVolume) {
      webapi.player_output_volume(this.output.id, newVolume)
    },

    set_enabled: function () {
      const values = {
        selected: !this.output.selected
      }
      webapi.output_update(this.output.id, values)
    }
  }
}
</script>

<style></style>
