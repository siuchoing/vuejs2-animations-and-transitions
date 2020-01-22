<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Animations</h1>
                <hr>
                <button class="btn btn-primary" @click="show = !show">Show Alert</button>
                <br><br>
                <!-- Only one element for transition-->
                <transition name="fade">
                    <div class="alert alert-info" v-if="show">This is some Info</div>
                </transition>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                show: false
            }
        }
    }
</script>

/********************************************
* .fade-enter: is attached for one frame at the beginning
* .fade-enter-active: is attached for the whole, at each element in animation time
* .fade where default opacity = 1
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
/* ---------- End of with named transition ---------- */
</style>
