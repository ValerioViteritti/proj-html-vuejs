<script>
export default {
    name: 'HealthIsWealth',
    props: {
        imageSrc: {
            type: String,
            required: true
        },
        inverse: {
            type: Boolean,
            default: false
        },
        showContactInfo: {
            type: Boolean,
            default: true
        }
    },
    data() {
        return {
            isAnimated: false
        };
    },
    mounted() {
        this.animateHeading();
    },
    methods: {
        animateHeading() {
            this.isAnimated = true;
            setTimeout(() => {
                this.isAnimated = false;
            }, 100);
        },
        goToAboutUs() {
            
        }
    }
}
</script>

<template>
    <div class="container">
        <div class="row" >
            <div class="col" :class="{ 'inverse-row': inverse }">
                <div class="box">
                    <span>Running week top selling</span>
                    <h2 :class="{ animate: isAnimated }">Health is wealth - eat <span>natural,</span> eat <span>organic,</span> be healthy.</h2>
                    <p>Organic is loaded with a sense of rightness, with a set of rules. I would much rather someone bought food that was local and sustainable but not organic than bought organic food that had to be shipped across the world.</p>
                    <div class="d-flex">
                        <div>
                            <h4>All are handmade</h4>
                            <p>If I'm honest, the thing I am proudest of is my varieties of wild flowers in the hay meadow.</p>
                        </div>
                        <div>
                            <h4>100% Fresh organic</h4>
                            <p>If I'm honest, the thing I am proudest of is my varieties of wild flowers in the hay meadow.</p>
                        </div>
                    </div>
                    <div v-if="showContactInfo">
                        <h4>Call us for order: <span>+088012345 678 906</span></h4>
                        <p>Organic is loaded with a sense of rightness, with a set of rules. I would much rather someone bought food that was local and sustainable but not organic</p>
                    </div>
                    <button id="btn" class="button orange_bg" @click="goToAboutUs">About Us</button>
                </div>
                <div>
                    <img :src="imageSrc" alt="">
                </div>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
@use '/src/style/general.scss' as *;
@use '../style/partials/variables' as *;

.container {
    .row {
        .col {
            width: 80%;
            margin: 0 auto;

            // flex
            display: flex;
            justify-content: space-between;
            align-items: center;
            gap: 20px;
            
            &.inverse-row {
                flex-direction: row-reverse;
            }

            div.box {
                width: 52%;
                display: flex;
                flex-direction: column;
                align-items: baseline;
                gap: 10px;

                &>span:first-child{
                    font-size: 1.3rem;
                }

                span {
                    color: $orange;
                    font-weight: 600;
                }

                h2 {
                    font-size: 3.5rem;
                    font-weight: 600;
                    line-height: 58px;
                    transition: font-size 0.8s ease;

                }
                .animate {
                    font-size: 1.5rem;
                }

                p {
                    color: rgb(135, 135, 135);
                }

                h4 {
                    margin-bottom: 10px;
                    font-size: 1.4rem;
                }

                #btn {
                    width: 150px;
                    margin-top: 30px;
                    // transition: background-color $time;

                    &:hover{
                        background-color: $green;
                    }
                }

                .d-flex {
                    display: flex;
                    justify-content: space-between;
                    gap: 60px;
                    width: 100%;

                    div {
                        width: 50%;

                        &:nth-child(1), &:nth-child(2) {
                            margin-top: 20px;
                            line-height: 30px;
                        }
                    }
                }
            }

            div {
                width: 47%;
                img {
                    height: 100%;
                    width: 100%;
                }
            }
        }
    }
}
</style>
