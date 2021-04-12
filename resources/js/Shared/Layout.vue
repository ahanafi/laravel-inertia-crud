<template>
    <div>
    <navbar />
        <div class="container mt-4">
            <slot />
        </div>
    </div>
</template>

<script>
import Navbar from "./Navbar";
import FlashMessage from "./FlashMessage";
export default {
    name: "Layout",
    components: {
        Navbar, FlashMessage
    }
}
</script>

<style scoped>

</style>
