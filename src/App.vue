<script setup lang="ts">
import BannerComponent from './components/BannerComponent.vue'

// Example data for different banner modes
const squareItems = [
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=1',
    link: 'https://example.com/link1'
  },
  {
    type: 'cta',
    title: 'Special Offer',
    button: 'Shop Now',
    link: 'https://example.com/link2',
    backgroundColor: '#10B981'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=2',
    link: 'https://example.com/link3'
  }
] as any

const carouselItems = [
  {
    type: 'cta',
    title: 'New Collection',
    button: 'Explore',
    link: 'https://example.com/link1',
    backgroundColor: '#8B5CF6'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=3',
    link: 'https://example.com/link2'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=4',
    link: 'https://example.com/link3'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=5',
    link: 'https://example.com/link4'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=6',
    link: 'https://example.com/link5'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=7',
    link: 'https://example.com/link6'
  }
] as any

const rectangleItems = [
  {
    type: 'image',
    aspectRatio: 'rectangle',
    src: 'https://picsum.photos/1200/400?random=8',
    link: 'https://example.com/link1'
  },
  {
    type: 'image',
    aspectRatio: 'square',
    src: 'https://picsum.photos/400?random=9',
    link: 'https://example.com/link1'
  },
  {
    type: 'cta',
    title: 'Limited Time',
    button: 'Get Started',
    link: 'https://example.com/link1'
  }
] as any
</script>

<template>
  <div class="min-h-screen bg-gray-50 py-8">
    <div class="max-w-6xl mx-auto px-4 space-y-12">
      <!-- Header -->
      <div class="text-center">
        <h1 class="text-4xl font-bold text-gray-900 mb-4">Banner Component Examples</h1>
        <p class="text-lg text-gray-600">Demonstrating different modes and configurations</p>
      </div>

      <!-- Square Mode - Basic -->
      <section class="space-y-4">
        <div class="text-center">
          <h2 class="text-2xl font-semibold text-gray-800 mb-2">Square Mode - Basic Grid</h2>
          <p class="text-gray-600">3 boxes per row on desktop, stacked on mobile</p>
        </div>
        <BannerComponent
          mode="square"
          :carousel-on-mobile="false"
          :items="squareItems"
        />
      </section>

      <!-- Square Mode - With Carousel -->
      <section class="space-y-4">
        <div class="text-center">
          <h2 class="text-2xl font-semibold text-gray-800 mb-2">Square Mode - With Mobile Carousel</h2>
          <p class="text-gray-600">Grid on desktop, swipeable carousel with thumbnails on mobile</p>
        </div>
        <BannerComponent
          mode="square"
          :carousel-on-mobile="true"
          :items="carouselItems"
        />
      </section>

      <!-- Rectangle Mode -->
      <section class="space-y-4">
        <div class="text-center">
          <h2 class="text-2xl font-semibold text-gray-800 mb-2">Rectangle Mode</h2>
          <p class="text-gray-600">Full-width rectangle on desktop, square image with CTA below on mobile</p>
        </div>
        <BannerComponent
          mode="rectangle"
          :carousel-on-mobile="false"
          :items="rectangleItems"
        />
      </section>

      <!-- Additional Examples -->
      <section class="space-y-8">
        <div class="text-center">
          <h2 class="text-2xl font-semibold text-gray-800 mb-2">Additional Variations</h2>
        </div>

        <!-- CTA First -->
        <div class="space-y-2">
          <h3 class="text-lg font-medium text-gray-700">CTA Box First</h3>
          <BannerComponent
            mode="square"
            :carousel-on-mobile="false"
            :items="[
              {
                type: 'cta',
                title: 'Featured Deal',
                button: 'Learn More',
                link: 'https://example.com/featured',
                backgroundColor: '#EF4444'
              },
              {
                type: 'image',
                aspectRatio: 'square',
                src: 'https://picsum.photos/400?random=10',
                link: 'https://example.com/image1'
              },
              {
                type: 'image',
                aspectRatio: 'square',
                src: 'https://picsum.photos/400?random=11',
                link: 'https://example.com/image2'
              }
            ]"
          />
        </div>

        <!-- CTA Last -->
        <div class="space-y-2">
          <h3 class="text-lg font-medium text-gray-700">CTA Box Last</h3>
          <BannerComponent
            mode="square"
            :carousel-on-mobile="false"
            :items="[
              {
                type: 'image',
                aspectRatio: 'square',
                src: 'https://picsum.photos/400?random=12',
                link: 'https://example.com/image3'
              },
              {
                type: 'image',
                aspectRatio: 'square',
                src: 'https://picsum.photos/400?random=13',
                link: 'https://example.com/image4'
              },
              {
                type: 'cta',
                title: 'Join Now',
                button: 'Sign Up',
                link: 'https://example.com/signup',
                backgroundColor: '#F59E0B'
              }
            ]"
          />
        </div>
      </section>

      <!-- Instructions -->
      <section class="bg-white rounded-lg p-6 shadow-sm">
        <h3 class="text-lg font-semibold text-gray-800 mb-4">How to Test</h3>
        <ul class="space-y-2 text-gray-600">
          <li>• <strong>Desktop:</strong> Resize your browser to see the 3-column grid layout</li>
          <li>• <strong>Mobile:</strong> Resize to mobile width to see stacked layout and carousel functionality</li>
          <li>• <strong>Interactions:</strong> Click on images and CTA boxes to test link functionality</li>
          <li>• <strong>Carousel:</strong> On mobile, swipe the main image and tap thumbnails in carousel mode</li>
          <li>• <strong>Aspect Ratios:</strong> Notice how images maintain their square aspect ratio when resized</li>
        </ul>
      </section>
    </div>
  </div>
</template>

<style scoped>
/* Additional styles for the demo page */
</style>
