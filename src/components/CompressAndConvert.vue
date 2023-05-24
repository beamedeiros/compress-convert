<template>
    <div>
        <input type="file" ref="fileInput" accept="image/*" @change="handleFileChange" />
        <button @click="compressAndDownload">Comprimir e baixar em WebP</button>
    </div>
</template>
  
<script>
export default {
    name:'compressAndConvert',
    data() {
        return {
            compressedImage: null
        };
    },
    methods: {
        handleFileChange(event) {
            const file = event.target.files[0];
            this.compressImage(file);
        },
        compressImage(file) {
            const reader = new FileReader();

            reader.onload = (event) => {
                const img = new Image();

                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    const ctx = canvas.getContext('2d');

                    const maxWidth = 800;
                    const maxHeight = 600;
                    let width = img.width;
                    let height = img.height;

                    if (width > height) {
                        if (width > maxWidth) {
                            height *= maxWidth / width;
                            width = maxWidth;
                        }
                    } else {
                        if (height > maxHeight) {
                            width *= maxHeight / height;
                            height = maxHeight;
                        }
                    }

                    canvas.width = width;
                    canvas.height = height;

                    ctx.drawImage(img, 0, 0, width, height);

                    canvas.toBlob((blob) => {
                        this.compressedImage = new File([blob], file.name, { type: 'image/webp' });
                    }, 'image/webp', 0.7); //1 melhora a qualidade mas aumenta o tamanho da imagem
                };

                img.src = event.target.result;
            };

            reader.readAsDataURL(file);
        },
        compressAndDownload() {
            if (this.compressedImage) {
                const link = document.createElement('a');
                link.href = URL.createObjectURL(this.compressedImage);
                link.download = 'compressed_image.webp';
                link.click();
            }
        }
    }
};
</script>
  