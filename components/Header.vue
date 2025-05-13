<script setup lang="ts">
const header = useTemplateRef('header')
const toggleMobileMenu = () => {
    header.value?.classList.toggle("header--menu-open");
}

onMounted(() => {
    window.addEventListener('scroll', () => {
        if (window.scrollY > 50) {
            header.value?.classList.add('header--scrolled');
        } else {
            header.value?.classList.remove('header--scrolled');
        }
    })
})
</script>

<template>
    <header ref="header" class="header">
        <div class="header-container">
            <div class="header__content">
                <a href="#" class="header__logo">Kaede</a>

                <nav class="header__desktop-nav">
                    <ul>
                        <li><a href="#">Home</a></li>
                        <li><a href="#">About</a></li>
                        <li><a href="#">Projects</a></li>
                        <li><a href="#">Contact</a></li>
                    </ul>
                </nav>
                <button class="header__mobile-menu-btn" aria-label="Toggle menu" @click="toggleMobileMenu">
                    <span></span>
                    <span></span>
                    <span></span>
                </button>
            </div>

            <nav class="header__mobile-nav">
                <ul>
                    <li><a href="#" @click="toggleMobileMenu">Home</a></li>
                    <li><a href="#" @click="toggleMobileMenu">About</a></li>
                    <li><a href="#" @click="toggleMobileMenu">Projects</a></li>
                    <li><a href="#" @click="toggleMobileMenu">Contact</a></li>
                </ul>
            </nav>
        </div>
    </header>
</template>