<template>
    <div class="tabs">
        <slot></slot>
    </div>
</template>

<script>
    import Vue from 'vue';

    export default {
        name: "w-tabs",
        props: {
            selected: {
                type: String,
                required: true
            },
            orientation: {
                type: String,
                default: 'horizontal',
                validator(value) {
                    return ['horizontal', 'vertical'].indexOf(value) >= 0;
                }
            }
        },
        data() {
            return {
                eventBus: new Vue()
            }
        },
        provide() {
            return {
                eventBus: this.eventBus
            }
        },
        methods: {
            checkChildren() {
                if (this.$children.length === 0) {
                    console && console.warn &&
                    console.warn('Children of Tabs should be TabsHead and TabsBody!');
                }
            },
            selectTab() {
                this.$children.forEach((vm) => {
                    if (vm.$options.name === 'w-tabs-head') {
                        vm.$children.forEach((item) => {
                            if (item.$options.name === 'w-tabs-item' && item.name === this.selected) {
                                this.eventBus.$emit('update:selected', this.selected, item);
                            }
                        });
                    }
                });
            }
        },
        mounted() {
            this.checkChildren();
            this.selectTab();
        }
    }
</script>

<style scoped lang="scss">
    .tabs {
    }
</style>