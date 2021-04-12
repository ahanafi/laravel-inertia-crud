<template>
    <div>
        <div class="mb-3">
            <div v-if="$page.props.flash.message" class="alert alert-success" role="alert">
                {{ $page.props.flash.message }}
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "FlashMessage"
}
</script>

<style scoped>

</style>
