<template>
  <div
    class="burger"
    @click="toggleDrawer"
    :class="{ open: drawerOpen }"
    aria-label="Toggle menu"
    role="button"
    tabindex="0"
  >
    <svg
      width="30"
      height="30"
      viewBox="0 0 24 24"
      fill="none"
      stroke="white"
      stroke-width="2"
      stroke-linecap="round"
      stroke-linejoin="round"
      aria-hidden="true"
    >
      <line class="line1" x1="3" y1="7" x2="21" y2="7" />
      <line class="line2" x1="3" y1="12" x2="21" y2="12" />
      <line class="line3" x1="3" y1="17" x2="21" y2="17" />
    </svg>
  </div>

  <div v-show="drawerOpen" class="overlay" @click="toggleDrawer"></div>

  <div v-show="drawerOpen" class="drawer">
    <nav>
      <NuxtLink :to="'/'">Реализованные проекты</NuxtLink>
      <NuxtLink :to="'/news'">Новости</NuxtLink>
      <NuxtLink :to="'/contacts'">Контакты</NuxtLink>
    </nav>

    <div class="drawer__contacts">
      <a :href="'tel:' + contacts.phone">{{ contacts.phone }}</a>
      <a>{{ contacts.email }}</a>
      <a>{{ contacts.address }}</a>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onBeforeUnmount, watch } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import { contacts } from '~/data/contacts';

const router = useRouter();
const route = useRoute();

const drawerOpen = ref<boolean>(false);

const toggleDrawer = (): void => {
  drawerOpen.value = !drawerOpen.value;
};

const handleResize = (): void => {
  if (window.innerWidth > 992) {
    drawerOpen.value = false;
  }
};

onMounted(() => {
  window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize);
});

watch(drawerOpen, (isOpen) => {
  if (isOpen) {
    document.body.style.overflow = 'hidden';
  } else {
    document.body.style.overflow = 'visible';
  }
});
</script>

<style lang="scss" scoped>
.burger {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  width: 50px;
  height: 50px;
  background: $color-primary;
  border-radius: 30%;
  z-index: 100;

  svg {
    stroke: white;
  }

  line {
    transition: all 0.3s ease-in-out;
  }

  &.open {
    line.line1 {
      transform: translateY(5px) rotate(45deg);
      transform-origin: center;
    }
    line.line2 {
      opacity: 0;
    }
    line.line3 {
      transform: translateY(-5px) rotate(-45deg);
      transform-origin: center;
    }
  }
}

.drawer {
  position: fixed;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  top: 0;
  right: 0;
  height: 100%;
  background: $color-primary;
  width: 65%;
  z-index: 99;
  padding: 80px 20px 20px 20px;
  transition: transform 0.2s ease-in-out;

  nav {
    display: flex;
    flex-direction: column;

    a {
      cursor: pointer;
      text-decoration: none;
      color: #fff;
      margin: 3% 0;
      font-family: $font-secondary;
      font-weight: 400;
      font-size: 18px;
      line-height: 120%;
      transition: 0.3s;

      &:hover {
        opacity: 0.6;
      }
    }
  }

  &__contacts {
    margin-top: auto;
    display: flex;
    flex-direction: column;

    a {
      cursor: pointer;
      text-decoration: none;
      color: #fff;
      margin: 3% 0;
      display: flex;
      align-items: center;
      font-family: $font-primary;
      font-weight: 400;
      font-size: 14px;
      transition: 0.3s;

      &:hover {
        opacity: 0.6;
      }
    }

    a:nth-child(1)::before {
      content: '';
      display: inline-block;
      width: 16px;
      height: 16px;
      background-color: #fff;
      mask: url('/img/icons/telephone.svg') no-repeat center;
      mask-size: 16px 16px;
      margin-right: 10px;
    }

    a:nth-child(2)::before {
      content: '';
      background-color: #fff;
      mask: url('/img/icons/email.svg') no-repeat center;
      mask-size: 16px 16px;
      width: 16px;
      height: 16px;
      display: inline-block;
      margin-right: 10px;
    }

    a:nth-child(3)::before {
      content: '';
      background-color: #fff;
      mask: url('/img/icons/location.svg') no-repeat center;
      mask-size: 16px 16px;
      width: 16px;
      height: 16px;
      display: inline-block;
      margin-right: 10px;
    }
  }
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.4);
  z-index: 90;
  transition: opacity 0.3s ease-in-out;
}
</style>
