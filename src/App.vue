<template>
    <main id="app">
        <Board />
        <section id="feedbacks">
            <Feedback
                v-for="turn in total"
                v-bind:class="{ active: isActive(total - turn) }"
                v-bind:key="total - turn + 1"
                v-bind:fb="feedback[total - turn + 1]"
            />
        </section>
        <Check v-bind:style="{ transform: checkButtonTranslation }" />
        <Dialog v-if="won" status="happy" />
        <Dialog v-if="lost" status="sad" />
        <Help @gotHelp="showHelp = false" v-bind:class="{ active: showHelp }" />
    </main>
</template>

<script>
import { mapGetters, mapState, mapActions } from 'vuex';

import Board from './components/Board.vue';
import Feedback from './components/Feedback.vue';
import Check from './components/Check.vue';
import Dialog from './components/Dialog.vue';
import Help from './components/Help.vue';

export default {
    name: 'app',
    components: {
        Board,
        Feedback,
        Check,
        Dialog,
        Help,
    },
    data() {
        return {
            showHelp: true,
        };
    },
    computed: {
        ...mapGetters(['checkButtonTranslation', 'isActive']),
        ...mapState(['current', 'total', 'won', 'lost', 'feedback']),
    },
    methods: {
        ...mapActions(['generateSecret']),
    },
    mounted() {
        this.generateSecret();
    },
};
</script>

<style lang="scss">
@import './sass/reset';
@import './sass/settings';
@import './sass/utils';
@import './sass/main';

#app {
    @include limit-to(290px);
    position: relative;
    z-index: 1;
}

#feedbacks {
    @include limit-to($h + ($aria * 2));
    @include main-shadow;
    background-color: $sand;
    border-radius: 0 $bd $bd 0;
    padding-bottom: $aria;
    position: absolute;
    top: $aria + $h;
    right: 0;
    z-index: -1;
}
</style>
