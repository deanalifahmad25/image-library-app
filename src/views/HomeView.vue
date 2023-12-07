<script setup>
    import ImageUpload from '../components/ImageUpload.vue'
</script>

<template>
    <main>
        <section class="h-100 w-100"
            style="box-sizing: border-box; background-image: linear-gradient(to bottom, rgba(31, 29, 43, 1), rgba(39, 37, 53, 1));">
            <div class="content" style="font-family: 'Poppins', sans-serif;">
                <div class="mx-auto d-flex align-items-center justify-content-center flex-column">
                    <div class="text-center w-100">
                        <div class="d-flex justify-content-center">
                            <ImageUpload />
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </main>
</template>
