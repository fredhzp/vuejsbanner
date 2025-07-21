<script setup lang="ts">
import { computed, ref } from 'vue'
import { Carousel, Slide } from 'vue3-carousel'
import 'vue3-carousel/carousel.css'

interface ImageItem {
  type: 'image'
  aspectRatio: 'square' | 'rectangle'
  src: string
  link: string
}

interface CTAItem {
  type: 'cta'
  title: string
  button: string
  link: string
  backgroundColor?: string
}

type BannerItem = ImageItem | CTAItem

interface Props {
  mode: 'square' | 'rectangle'
  carouselOnMobile?: boolean
  items: BannerItem[]
} 

const props = withDefaults(defineProps<Props>(), {
  carouselOnMobile: false
})

// Separate images and CTA for different rendering logic
const images = computed(() => props.items.filter(item => item.type === 'image') as ImageItem[])
const cta = computed(() => props.items.find(item => item.type === 'cta') as CTAItem | undefined)

// For rectangle mode, get desktop and mobile images
const desktopImage = computed(() => 
  images.value.find(img => img.aspectRatio === 'rectangle')
)
const mobileImage = computed(() => 
  images.value.find(img => img.aspectRatio === 'square') || images.value[0]
)

// Carousel configuration for mobile
const carouselConfig = {
  itemsToShow: 1,
  wrapAround: true,
  snapAlign: 'center' as const
}

const thumbnailCarouselConfig = {
  itemsToShow: 4,
  wrapAround: false,
  snapAlign: 'start' as const
}

// Reference to the main carousel
const mainCarousel = ref()

// Handle clicks
const handleItemClick = (link: string) => {
  if (link) {
    window.open(link, '_blank')
  }
}

// Handle thumbnail click to navigate main carousel
const handleThumbnailClick = (index: number) => {
  if (mainCarousel.value) {
    mainCarousel.value.slideTo(index)
  }
}
</script>

<template>
  <div class="banner-component">
    <!-- Square Mode -->
    <div v-if="mode === 'square'" class="square-mode">
      <!-- Desktop Layout -->
      <div class="hidden md:grid md:grid-cols-3 gap-4">
        <div
          v-for="(item, index) in items"
          :key="index"
          class="aspect-square cursor-pointer transition-transform hover:scale-105"
          @click="handleItemClick(item.link)"
        >
          <!-- Image Item -->
          <div
            v-if="item.type === 'image'"
            class="w-full h-full bg-gray-200 rounded-lg overflow-hidden"
          >
            <img
              :src="item.src"
              :alt="`Image ${index + 1}`"
              class="w-full h-full object-cover"
            />
          </div>
          
          <!-- CTA Item -->
          <div
            v-else-if="item.type === 'cta'"
            class="w-full h-full rounded-lg flex flex-col items-center justify-center text-white p-6"
            :style="{ backgroundColor: item.backgroundColor || '#3B82F6' }"
          >
            <h3 class="text-xl font-bold mb-4 text-center">{{ item.title }}</h3>
            <button class="px-6 py-2 border-2 border-white rounded-lg hover:bg-white hover:text-gray-800 transition-colors">
              {{ item.button }}
            </button>
          </div>
        </div>
      </div>

      <!-- Mobile Layout -->
      <div class="md:hidden">
        <!-- Mobile Carousel Mode -->
        <div v-if="carouselOnMobile && images.length > 1" class="mobile-carousel">
          <!-- Main Image Carousel -->
          <div class="mb-4">
            <Carousel ref="mainCarousel" v-bind="carouselConfig" class="main-carousel">
              <Slide v-for="(image, index) in images" :key="index">
                <div 
                  class="aspect-square cursor-pointer mx-2"
                  @click="handleItemClick(image.link)"
                >
                  <img
                    :src="image.src"
                    :alt="`Image ${index + 1}`"
                    class="w-full h-full object-cover rounded-lg"
                  />
                </div>
              </Slide>
            </Carousel>
          </div>

          <!-- Thumbnail Navigation -->
          <div class="thumbnails mb-4">
            <Carousel v-bind="thumbnailCarouselConfig" class="thumbnail-carousel">
              <Slide v-for="(image, index) in images" :key="index">
                <div class="px-1">
                  <img
                    :src="image.src"
                    :alt="`Thumbnail ${index + 1}`"
                    class="w-16 h-16 object-cover rounded cursor-pointer opacity-70 hover:opacity-100 transition-opacity"
                    @click="handleThumbnailClick(index)"
                  />
                </div>
              </Slide>
            </Carousel>
          </div>

          <!-- CTA Box (narrow rectangle on mobile) -->
          <div
            v-if="cta"
            class="w-full h-24 rounded-lg flex items-center justify-between px-6 cursor-pointer transition-transform hover:scale-105"
            :style="{ backgroundColor: cta.backgroundColor || '#3B82F6' }"
            @click="handleItemClick(cta.link)"
          >
            <h3 class="text-lg font-bold text-white">{{ cta.title }}</h3>
            <button class="px-4 py-2 border-2 border-white rounded text-white hover:bg-white hover:text-gray-800 transition-colors">
              {{ cta.button }}
            </button>
          </div>
        </div>

        <!-- Mobile Grid Mode (no carousel) -->
        <div v-else class="grid grid-cols-1 gap-4">
          <div
            v-for="(item, index) in items"
            :key="index"
            class="cursor-pointer transition-transform hover:scale-105"
            @click="handleItemClick(item.link)"
          >
            <!-- Image Item (square) -->
            <div
              v-if="item.type === 'image'"
              class="aspect-square w-full bg-gray-200 rounded-lg overflow-hidden"
            >
              <img
                :src="item.src"
                :alt="`Image ${index + 1}`"
                class="w-full h-full object-cover"
              />
            </div>
            
            <!-- CTA Item (narrow rectangle on mobile) -->
            <div
              v-else-if="item.type === 'cta'"
              class="w-full h-24 rounded-lg flex items-center justify-between px-6 text-white"
              :style="{ backgroundColor: item.backgroundColor || '#3B82F6' }"
            >
              <h3 class="text-lg font-bold">{{ item.title }}</h3>
              <button class="px-4 py-2 border-2 border-white rounded hover:bg-white hover:text-gray-800 transition-colors">
                {{ item.button }}
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Rectangle Mode -->
    <div v-else-if="mode === 'rectangle'" class="rectangle-mode">
      <!-- Desktop Layout -->
      <div class="hidden md:block relative">
        <div
          v-if="desktopImage"
          class="relative w-full h-96 bg-gray-200 rounded-lg overflow-hidden cursor-pointer"
          @click="handleItemClick(desktopImage.link)"
        >
          <img
            :src="desktopImage.src"
            :alt="'Desktop banner image'"
            class="w-full h-full object-cover"
          />
          
          <!-- CTA Box overlay (hovering on top of image, right-aligned, full height) -->
          <div
            v-if="cta"
            class="absolute top-0 right-0 h-full w-80 bg-white/20 bg-opacity-10 backdrop-blur-sm flex flex-col items-center justify-center p-8 cursor-pointer"
            @click.stop="handleItemClick(cta.link)"
          >
            <h3 class="text-2xl font-bold mb-6 text-white text-center">{{ cta.title }}</h3>
            <button class="px-8 py-3 border-2 border-white text-white rounded-lg hover:bg-white hover:text-gray-800 transition-colors font-medium">
              {{ cta.button }}
            </button>
          </div>
        </div>
      </div>

      <!-- Mobile Layout -->
      <div class="md:hidden">
        <!-- Square Image -->
        <div
          v-if="mobileImage"
          class="aspect-square w-full bg-gray-200 rounded-lg overflow-hidden mb-4 cursor-pointer"
          @click="handleItemClick(mobileImage.link)"
        >
          <img
            :src="mobileImage.src"
            :alt="'Mobile banner image'"
            class="w-full h-full object-cover"
          />
        </div>

        <!-- CTA Box below image (white background) -->
        <div
          v-if="cta"
          class="w-full h-24 rounded-lg flex items-center justify-between px-6 text-white"
          :style="{ backgroundColor: cta.backgroundColor || '#3B82F6' }"
          @click="handleItemClick(cta.link)"
        >
          <h3 class="text-lg font-bold">{{ cta.title }}</h3>
          <button class="px-4 py-2 border-2 border-white rounded hover:bg-white hover:text-gray-800 transition-colors">
            {{ cta.button }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

