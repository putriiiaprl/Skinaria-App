<template>
  <header class="header">
    <nav class="nav container">
      <div class="logo">{{ appName }}</div>
      <ul class="nav-links" :class="{ 'nav-active': isMenuOpen }">
        <li v-for="link in navLinks" :key="link.href">
          <a :href="link.href" @click="closeMenu">{{ link.text }}</a>
        </li>
      </ul>
      <div class="hamburger" @click="toggleMenu">
        <span></span>
        <span></span>
        <span></span>
      </div>
    </nav>
  </header>
</template>

<script>
export default {
  name: "TheHeader",
  data() {
    return {
      appName: "Skinoria",
      isMenuOpen: false,
      navLinks: [
        { href: "#beranda", text: "Beranda" },
        { href: "#tentang", text: "Tentang" },
        { href: "#cara-kerja", text: "Cara Kerja" },
        { href: "#artikel", text: "Artikel" },
      ],
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
    closeMenu() {
      this.isMenuOpen = false;
    },
  },
};
</script>

<style scoped>
.hamburger {
  display: none;
  flex-direction: column;
  cursor: pointer;
}

.hamburger span {
  width: 25px;
  height: 3px;
  background: #4a90e2;
  margin: 3px 0;
  transition: 0.3s;
}

@media (max-width: 768px) {
  .nav-links {
    position: fixed;
    top: 70px;
    right: -100%;
    width: 100%;
    height: calc(100vh - 70px);
    background: white;
    flex-direction: column;
    justify-content: start;
    align-items: center;
    padding-top: 2rem;
    transition: right 0.3s;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }

  .nav-links.nav-active {
    right: 0;
  }

  .hamburger {
    display: flex;
  }
}
</style>
