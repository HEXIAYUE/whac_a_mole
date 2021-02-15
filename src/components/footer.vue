<template>
    <div class="footer">
        禾下月/<a href="https://hxyxyz.top">小小窝</a>
    </div>
</template>

<script>
export default {
    name: 'footer'
};
</script>

<style scope lang="scss">
.footer {
    width: 100%;
    height: 25px;
    text-align: center;
    color: #3a3a3a;
    font-size: 20px;

    a {
        color: #3a3a3a;
        text-decoration: none;

        &:hover {
            color: #ec0c38;
        }
    }
}
</style>
