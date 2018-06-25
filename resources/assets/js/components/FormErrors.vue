<template>
    <span style="display: block;">
        <slot></slot>
    </span>
</template>

<script>
    export default {
        props: ['errors'],
    }
</script>