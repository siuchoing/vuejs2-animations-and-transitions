<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <select v-model="alertAnimation" class="form-control">
                    <option value="fade">Fade</option>
                    <option value="slide">Slide</option>
                </select>
                <br><br>
                <button class="btn btn-primary" @click="show = !show">Show Alert</button>
                <br><br>
                <!-- Only one element for transition, default transition type-->
                <transition :name="alertAnimation">
                    <div class="alert alert-info" v-show="show">Fade transition</div>
                </transition>
                <!-- Add animation type to prevent css conflict -->
                <transition name="slide" type="animation" appear>
                    <div class="alert alert-info" v-if="show">Fade and Slide Animation</div>
                </transition>
                <!-- add the appear attribute to do this smoothly animates -->
                <transition
                        enter-active-class="animated bounce"
                        leave-active-class="animated shake"
                >
                    <div class="alert alert-info" v-if="show">Bounce and Shake Animation</div>
                </transition>
                <!-- out-in: let old element animate out first and then animate in the new one; -->
                <!-- in-out: does the opposite -->
                <!-- if reload page, this bottom div now first fades out before the new one fades -->
                <transition :name="alertAnimation" mode="out-in">
                    <!-- adding unique key to avoid jumping -->
                    <div class="alert alert-info" v-if="show" key="info">This is info message</div>
                    <div class="alert alert-warning" v-else key="warning">This is warning message</div>
                </transition>
                <hr>
                <button class="btn btn-primary" @click="load = !load">Load / Remove Element</button>
                <br><br>
                <!-- :css="false" means don't look for css classes, to skip hook step; and "false" as boolean type -->
                <transition
                    @before-enter="beforeEnter"
                    @enter="enter"
                    @after-enter="afterEnter"
                    @enter-cancelled="enterCancelled"

                    @before-leave="beforeLeave"
                    @leave="leave"
                    @after-leave="afterLeave"
                    @leave-cancelled="leaveCancelled"
                    :css="false"
                >
                    <div style="width: 300px; height: 100px; background-color: lightgreen;" v-if="load"></div>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show: false,
                load: true,
                alertAnimation: 'fade',
                elementWidth: 100
            }
        },
        methods: {
        // Add element hook
            beforeEnter(el) {
                console.log('beforeEnter');
                this.elementWidth = 100;
                el.style.width = this.elementWidth + 'px';
            },
            enter(el, done) {
                console.log('enter');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth + round * 10) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);
            },
            afterEnter() {
                console.log('afterEnter');
            },
            enterCancelled() {
                console.log('enterCancelled');
            },
        // Remove element hook
            beforeLeave(el) {
                console.log('beforeLeave');
                this.elementWidth = 300;
                el.style.width = this.elementWidth + 'px';
            },
            leave(el, done) {
                console.log('leave');
                let round = 1;
                const interval = setInterval(() => {
                    el.style.width = (this.elementWidth - round * 10) + 'px';
                    round++;
                    if (round > 20) {
                        clearInterval(interval);
                        done();
                    }
                }, 20);
            },
            afterLeave(el) {
                console.log('afterLeave');
            },
            leaveCancelled(el) {
                console.log('leaveCancelled');
            }
        }
    }
</script>

/********************************************
* .fade-enter: is attached for one frame at the beginning
* .fade-enter-active: is attached for the whole, at each element in animation time
* .fade where default opacity = 1
* .ease-out: end a bit slower
* .forwards: let element stays in finishing position
********************************************/
<style>
/* ---------- With named transition ---------- */
    /* transparent when fade */
    .fade-enter {
        opacity: 0;
    }

    /* non-transparent duration */
    .fade-enter-active {
        transition: opacity 1s;
        /*opacity = 1 (default)*/
    }

    /*non-transparent*/
    .fade-leave {
        /*opacity = 1 (default)*/
    }

    .fade-leave-active {
        transition: opacity 1s;
        opacity: 0;
    }

    .slide-enter {
        opacity: 0;
        /*transform: translateY(20px);*/
    }

    .slide-enter-active {
        animation: slide-in 1s ease-out forwards;
        transition: opacity .5s;
    }

    .slide-leave-active {
        animation: slide-out 1s ease-out forwards;
        transition: opacity 1s;
        opacity: 0;
    }

    @keyframes slide-in {
        from {
            transform: translateY(20px);
        }
        to {
            transform: translateY(0);
        }
    }

    @keyframes slide-out {
        from {
            transform: translateY(0);
        }
        to {
            transform: translateY(20px);
        }
    }
/* ---------- End of with named transition ---------- */
</style>
