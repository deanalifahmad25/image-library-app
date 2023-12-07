<template>
    <div class="container col-md-8">
        <div class="mb-5">
            <h2 class="title-text text-white">Upload Image</h2>
            <input class="form-control btn-set" type="file" @change="uploadImage" accept="image/*">
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
    import Swal from 'sweetalert2';

    export default {
        data() {
            return {
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
                        this.showAlert('Hanya boleh file gambar', 'error');
                        return;
                    }

                    const maxSize = 2 * 1024 * 1024;
                    if (file.size > maxSize) {
                        this.showAlert('File gambar terlalu besar', 'error');
                        return;
                    }

                    const formData = new FormData();
                    formData.append('image', file);

                    const apiKey = '2fcf4aadcbdce7584a7687e0171a316c';
                    const apiUrl = `https://api.imgbb.com/1/upload?key=${apiKey}`;

                    axios.post(apiUrl, formData)
                        .then(response => {
                            this.imageUrl = response.data.data.url;
                            this.showAlert('Gambar berhasil diupload!');

                            const uploadedImages = JSON.parse(localStorage.getItem('uploadedImages')) || [];
                            uploadedImages.push(this.imageUrl);
                            localStorage.setItem('uploadedImages', JSON.stringify(uploadedImages));

                            this.uploadedImages = uploadedImages;
                        })
                        .catch(error => {
                            console.error('Upload error:', error);
                            this.showAlert('Upload error. Silahkan coba lagi.', 'error');
                        });
                } else {
                    this.showAlert('Silahkan pilih file gambar.');
                }
            },
            showAlert(message, icon = 'success') {
                Swal.fire({
                    icon,
                    title: icon === 'error' ? 'Error!' : 'Berhasil!',
                    text: message,
                });
            },
        },
        mounted() {
            const uploadedImages = JSON.parse(localStorage.getItem('uploadedImages')) || [];
            this.uploadedImages = uploadedImages;
        },
    };
</script>
