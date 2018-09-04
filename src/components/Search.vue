<template>
    <div class="grid-x">
        <div class="cell small-12">
            <input type="input" class="searchInput" placeholder="Search..." @input="searchUpdate" />
        </div>
    </div>
</template>
<script>
export default {
    methods: {
        searchUpdate(e) {
            this.$emit('inputChanged', e.target.value);
        },
    },
};
</script>
<style lang="scss" scoped>
.searchInput {
    width: 100%;
    padding: 1em;
    border-radius: 3px;
    border: 1px solid rgb(214, 214, 214);
    font-size: 1.5em;
}
</style>
