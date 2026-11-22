<template>
    <div class="container-fluid">
        <div class="row">
            <div v-if="!$root.isMobile" class="col-12 col-md-4 col-xl-3">
                <div>
                    <router-link to="/compose" class="btn btn-primary mb-3"><font-awesome-icon icon="plus" /> {{ $t("compose") }}</router-link>
                </div>
                <StackList :scrollbar="true" />
            </div>

            <!-- Phones: the stack list lives in a drawer opened from the header -->
            <BOffcanvas v-else v-model="$root.stackMenuOpen" placement="start" class="stack-menu">
                <template #header="{ hide }">
                    <router-link to="/compose" class="btn btn-primary" @click="hide()"><font-awesome-icon icon="plus" /> {{ $t("compose") }}</router-link>
                    <button type="button" class="btn-close" aria-label="Close" @click="hide()"></button>
                </template>
                <!-- Any link in the list leads to a page, so picking one closes the drawer -->
                <div @click="closeMenuOnLink">
                    <StackList :scrollbar="false" />
                </div>
            </BOffcanvas>

            <div ref="container" class="col-12 col-md-8 col-xl-9 mb-3">
                <!-- Add :key to disable vue router re-use the same component -->
                <router-view :key="$route.fullPath" :calculatedHeight="height" />
            </div>
        </div>
    </div>
</template>

<script>

import StackList from "../components/StackList.vue";

export default {
    components: {
        StackList,
    },
    data() {
        return {
            height: 0
        };
    },
    mounted() {
        this.height = this.$refs.container.offsetHeight;
    },
    methods: {
        closeMenuOnLink(event) {
            if (event.target.closest("a[href]")) {
                this.$root.stackMenuOpen = false;
            }
        },
    },
};
</script>

<style lang="scss" scoped>
.container-fluid {
    width: 98%;
}
</style>

<style lang="scss">
@import "../styles/vars.scss";

// Teleported to <body>, so it can't be scoped
.stack-menu {
    max-width: 85vw;

    .offcanvas-body {
        padding: 0.5rem;
    }

    .dark & {
        background-color: $dark-bg;
        color: $dark-font-color;

        .btn-close {
            filter: invert(1);
        }
    }
}
</style>
