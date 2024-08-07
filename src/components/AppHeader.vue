<script>
import { router } from '../router';

export default {
    name: 'AppHeader',
    data() {
        return {
            routes: router.options.routes.slice(0, 3), // Le prime 3 rotte del router
            isScrolled: false, // Controlla se la pagina è scorsa
            showScrollToTop: false, // Mostra il pulsante "scroll to top"
        };
    },
    mounted() {
        window.addEventListener('scroll', this.handleScroll); // Aggiungi l'evento scroll
    },
    beforeUnmount() {
        window.removeEventListener('scroll', this.handleScroll); // Rimuovi l'evento scroll
    },
    methods: {
        handleScroll() {
            this.isScrolled = window.scrollY > 50; // Verifica se la pagina è scorsa più di 50px
            this.showScrollToTop = window.scrollY > 200; // Mostra il pulsante "scroll to top" se scorsi più di 200px
        },
        scrollToTop() {
            window.scrollTo({
                top: 0,
                behavior: 'smooth' // Scrolla in modo fluido
            });
        }
    }
}
</script>

<template>
    <header>
        <!-- Sezione superiore -->
        <section class="green_bg">
            <div class="container_90">
                <div>
                    <a href="#" class="header_i">
                        <i class="fa-solid fa-location-dot"></i>
                        North baukhula, tala, USA
                    </a>
                    <a href="#" class="header_i">
                        <i class="fa-solid fa-envelope"></i>
                        demo@example.com
                    </a>
                </div>
                <div>
                    <a href="#" class="header_i">
                        <i class="fa-regular fa-clock"></i>
                        8.30AM-8.30PM
                    </a>
                    <a href="#" class="header_i">
                        <i class="fa-brands fa-facebook-f"></i>
                    </a>
                    <a href="#" class="header_i">
                        <i class="fa-brands fa-twitter"></i>
                    </a>
                    <a href="#" class="header_i">
                        <i class="fa-brands fa-instagram"></i>
                    </a>
                    <a href="#" class="header_i">
                        <i class="fa-brands fa-pinterest-p"></i>
                    </a>
                </div>
            </div>
        </section>

        <!-- Sezione principale del menu -->
        <section :class="{ fixed: true, scrolled: isScrolled }">
            <div class="container_90">
                <nav>
                    <a href="#">
                        <img src="../assets/logo.png" alt="logo">
                    </a>
                    <ul>
                        <li v-for="(route, i) in routes" :key="i">
                            <router-link :to="route.path">{{ route.name }}</router-link>
                        </li>
                    </ul>
                </nav>
                <div class="shop">
                    <a href="#" class="header_i">
                        <i class="fa-solid fa-magnifying-glass orange_bg"></i>
                    </a>
                    <a href="#" class="header_i">
                        <i class="fa-solid fa-cart-shopping"></i>
                    </a>
                    <a href="#" class="button orange_bg">ORDER NOW</a>
                </div>
            </div>
        </section>

        <!-- Pulsante "scroll to top" -->
        <a href="#" id="scroll-to-top" class="orange_bg" @click.prevent="scrollToTop" v-if="showScrollToTop">
            <i class="fa-solid fa-chevron-up"></i>
        </a>
    </header>
</template>

<style scoped lang="scss">
@use '../style/partials/variables' as *;

header {
    background-color: $white;
    height: 150px;

    #scroll-to-top {
        position: fixed;
        bottom: 20px;
        right: 20px;
        border: none;
        border-radius: 5px;
        padding: 10px 15px;
        cursor: pointer;
        z-index: 900;
    }

    section {
        padding: 10px 0;

        .container_90 {
            display: flex;
            justify-content: space-between;
            align-items: center;

            .header_i {
                padding: 0 10px;
            }

            nav {
                display: flex;
                align-items: center;
                gap: 20px;

                ul {
                    margin-left: 50px;
                    display: flex;
                    gap: 20px;

                    li {
                        text-transform: uppercase;
                        font-size: 18px;
                        transition: color $time;

                        &:hover {
                            color: $orange;
                        }
                    }
                }
            }

            .fa-magnifying-glass {
                padding: 10px;
                border-radius: 50%;
            }

            .fa-cart-shopping {
                position: absolute;
                padding: 10px;
                position: relative;
                padding: 10px;
                border-radius: 50%;
                color: $green;
                background-color: $white;
                margin-right: 15px;

                &::after {
                    content: '0';
                    position: absolute;
                    top: -5px;
                    right: -5px;
                    background-color: $orange;
                    color: $white;
                    padding: 5px;
                    border-radius: 50%;
                    font-size: small;
                }
            }

            .shop {
                & a:last-child {
                    transition: background-color $time;

                    &:hover {
                        background-color: $green;
                    }
                }
            }

            .fa-brands {
                transition: color $time;

                &:hover {
                    color: black;
                }
            }
        }
    }

    .fixed {
        width: 100%;
        position: fixed;
        top: 59px;
        left: 0;
        z-index: 900;

        &.scrolled {
            background: rgba($color: #000000, $alpha: 0.7); /* Colore di sfondo quando si scrolla */
            color: #fff; /* Cambia anche il colore del testo */
            top: 0;
        }
    }
}
</style>
