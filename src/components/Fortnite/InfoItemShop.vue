<template>
  <div>
    <q-dialog :maximized="true" v-model="infoItem">
      <q-card class="bg-dark text-white">
        <div class="row">
          <q-btn @click="yourCallBackFunction(true)" icon="arrow_back" flat dense color="primary" label="Voltar"
            class="col-auto q-ma-sm" />
        </div>
        <div v-for="(item, index) in data.items" :key="index">
          <!-- ITEM -->
          <div class="bg-info" v-if="index === 0">
            <CarouselImgShop :item="data" />
            <q-card-section class="bg-info">
              <!-- NOME -->
              <div class="row-12 justify-center text-center">
                <div v-if="data.items.length <= 3" class="text-h6">
                  {{ data.items[0].name }}
                </div>
                <div v-else class="text-h6">
                  Pacotão - {{ data.items[0].name }}
                </div>
                <div class="text-caption">
                  {{ item.description }}
                </div>
              </div>
              <!-- PRICES -->
              <div class="row justify-center text-center items-center">
                <div v-if="data.regularPrice != data.finalPrice" class="col-2">
                  <div class="text-h5 strike-through">
                    {{ data.regularPrice }}
                  </div>
                </div>
                <div class="col-2">
                  <div :class="data.regularPrice != data.finalPrice ? 'text-h6 text-right' : 'text-h6'">
                    {{ data.finalPrice }}
                  </div>
                </div>
                <img v-if="vbuckImage" style="width: 27px;" :src="vbuckImage">
              </div>
            </q-card-section>
          </div>
        </div>
        <!-- ITEMS -->
        <div v-if="data.items.length > 1" class="row-12 title text-white text-center text-h3 q-pa-md">
          Acompanha
        </div>
        <div v-for="(item, index) in data.items" :key="item.id" class="display-flex">
          <div v-if="index > -1" class="warp">
            <!-- IMG -->
            <q-img
              :style="'background: radial-gradient(circle, #fff -80%, ' + bgColor(data.newDisplayAsset.materialInstances[0].colors.Background_Color_B) + ' 70%)'"
              :src="item.images.featured != null ? item.images.featured : item.images.icon">
              <div class="absolute-bottom text-subtitle2 text-center">
                {{ item.name }}
                <!-- <q-btn class="full-width" color="primary" label="Visualizar" @click="show(item.id)"/> -->
              </div>
            </q-img>
          </div>
        </div>
        <q-card-section>
          <div class="text-h6 q-mt-sm q-mb-xs text-primary">Raridade:</div>
          <div class="text-caption text-white">
            {{ data.items[0].rarity.displayValue }}
          </div>
          <div class="text-h6 q-mt-sm q-mb-xs text-primary">Lançamento:</div>
          <div class="text-caption text-white">
            {{ data.items[0].introduction.text }}
          </div>
          <div v-if="data.items[0].shopHistory != null" class="text-h6 q-mt-sm q-mb-xs text-primary">Última aparição na
            loja:</div>
          <div v-if="data.items[0].shopHistory != null" class="text-caption text-white">
            {{ data.items[0].shopHistory[position] | formatarData }}
          </div>
        </q-card-section>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
import { Loading } from 'quasar'
import CarouselImgShop from 'components/Fortnite/CarouselImgShop.vue'

export default {
  name: 'InfoItemShop',
  props: ['info', 'infoItem', 'vbuckImage'],
  components: {
    CarouselImgShop
  },
  data() {
    return {
      data: this.info,
      position: 0,
      next: false,
    }
  },
  watch: {
    info() {
      this.data = this.info
    },
  },
  created() {
    Loading.show()
    if (this.data.items == undefined) {
      var arrObj = [
        this.data
      ];
      this.data.items = arrObj
      if (this.data.items[0].shopHistory != null) {
        this.position = this.data.items[0].shopHistory.length - 2
      }
    } else {
      if (this.data.items[0].shopHistory != null) {
        this.position = this.data.items[0].shopHistory.length - 2
      }
    }
    Loading.hide()
  },
  mounted() {
    //
  },
  beforeDestroy() {
    if (this.infoItem) {
      if (this.$router.currentRoute.path != '/fortnite-shop') {
        this.$router.push({
          path: '/fortnite-shop'
        })
      }
    }
  },
  methods: {
    bgColor(value) {
      let color
      switch (value) {
        case 'uncommon':
          color = 'grey'
          break;
        case 'common':
          color = 'green'
          break;
        case 'rare':
          color = 'blue'
          break;
        case 'epic':
          color = 'purple'
          break;
        case 'legendary':
          color = 'orange'
          break;

        default:
          color = value
          break;
      }

      if (color == undefined || color == null) {
        color = '#1492FF'
      } else {
        color = '#' + value
      }

      return color
    },
    back() {
      this.$emit('back', false)
    },
    show(id) {
      this.$router.push({
        path: '/cosmetic/' + id
      })
    },
    yourCallBackFunction(event = false) {
      if (event == true) {
        this.back()
      }
    }
  }
}
</script>
<style>
@import url('https://fonts.googleapis.com/css2?family=Anton&display=swap');

.title {
  font-family: 'Anton', sans-serif;
}

.display-flex {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.warp {
  padding: 10px;
  flex-basis: 50%;
  flex-grow: 1;
}
</style>