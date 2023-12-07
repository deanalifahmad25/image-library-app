<template>
    <div class="container col-md-8">
        <div class="mb-5">
            <h2 class="title-text text-white">Upload Image</h2>
            <input class="form-control btn-set" type="file" @change="uploadImage" accept="image/*">
            <p v-if="message">{{ message }}</p>
            <div class="card mx-auto my-3 card-item position-relative" style="width: 18rem;">
                <img v-if="imageUrl" :src="imageUrl" class="card-img-top" alt="">
            </div>
        </div>

        <div class="mb-5">
            <h2 class="title-text text-white">Gallery</h2>
            <div v-if="uploadedImages.length > 0" class="d-flex flex-wrap">
                <div v-for="(image, index) in uploadedImages" :key="index"
                    class="card mx-auto my-3 card-item position-relative" style="width: 18rem;">
                    <img :src="image" class="card-img-top" alt="">
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';

    export default {
        data() {
            return {
                message: '',
                imageUrl: '',
                uploadedImages: [],
            };
        },
        methods: {
            uploadImage(event) {
                const file = event.target.files[0];

                if (file) {
                    const allowedTypes = ['image/jpeg', 'image/png', 'image/bmp', 'image/gif'];
                    if (!allowedTypes.includes(file.type)) {
                        this.message = 'Hanya boleh file gambar';
                        return;
                    }

                    const maxSize = 2 * 1024 * 1024;
                    if (file.size > maxSize) {
                        this.message = 'File gambar terlalu besar';
                        return;
                    }

                    const formData = new FormData();
                    formData.append('image', file);

                    const apiKey = '2fcf4aadcbdce7584a7687e0171a316c';
                    const apiUrl = `https://api.imgbb.com/1/upload?key=${apiKey}`;

                    this.message = 'Uploading...';

                    axios.post(apiUrl, formData)
                        .then(response => {
                            this.message = 'Gambar berhasil diupload!';
                            this.imageUrl = response.data.data.url;

                            const uploadedImages = JSON.parse(localStorage.getItem('uploadedImages')) || [];
                            uploadedImages.push(this.imageUrl);
                            localStorage.setItem('uploadedImages', JSON.stringify(uploadedImages));

                            this.uploadedImages = uploadedImages;
                        })
                        .catch(error => {
                            console.error('Upload error:', error);
                            this.message = 'Upload error. Silahkan coba lagi.';
                        });
                } else {
                    this.message = 'Silahkan pilih file gambar.';
                }
            },
        },
        mounted() {
            const uploadedImages = JSON.parse(localStorage.getItem('uploadedImages')) || [];
            this.uploadedImages = uploadedImages;
        },
    };
</script>
