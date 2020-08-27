<template>
  <div class="home">
    <HeaderShayna /> 
    <HeroShayna />
    <WomenShayna />
    <InstaShayna />
    <LogoShayna />
  </div>
</template>

<script>
// @ is an alias to /src
import HeaderShayna from '@/components/HeaderShayna.vue';
import HeroShayna from '@/components/HeroShayna.vue';
import WomenShayna from '@/components/WomenShayna.vue';
import InstaShayna from '@/components/InstaShayna.vue';
import LogoShayna from '@/components/LogoShayna.vue';

export default {
  name: 'Home',
  components: {
    HeaderShayna,
    HeroShayna,
    WomenShayna,
    InstaShayna,
    LogoShayna
  }
}
</script>
