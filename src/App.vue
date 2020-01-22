<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <button class="btn btn-primary" @click="show = !show">Show Alert</button>
                <br><br>
                <!-- Only one element for transition, default transition type-->
                <transition name="fade">
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
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show: true
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
        transition: opacity 3s;
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
