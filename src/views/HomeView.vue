<template>
  <div class="home">
    <HeaderShayna />
    <HeroSection />  
    <WomanShayna /> 
    <InstaShayna />
    <PartnerShayna/>
    <FooterShayna/>
  </div>
</template>

<script>
  // @ is an alias to /src
  // import HelloWorld from '@/components/HelloWorld.vue'
  import HeaderShayna from '@/components/HeaderShayna.vue'
  import HeroSection from '@/components/HeroSection'
  import WomanShayna from '@/components/WomanShayna.vue'
  import InstaShayna from '@/components/InstaShayna.vue'
  import PartnerShayna from '@/components/PartnerShayna.vue'
  import FooterShayna from '@/components/FooterShayna.vue'
  export default {
    name: 'HomeView',
    components: {
    HeaderShayna,
    HeroSection,
    WomanShayna,
    InstaShayna,
    PartnerShayna,
    FooterShayna
}
  }
</script>