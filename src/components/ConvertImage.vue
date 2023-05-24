<template>
    <div>
        <input type="file" ref="fileInput" accept="image/*" @change="handleFileChange" />
        <button @click="convertAndDownload">Converter e baixar em WebP</button>
    </div>
</template>
  
<script>
export default {
    name:'ConvertImage',
    methods: {
        //recupera o arquivo selecionado
        handleFileChange(event) {
            const file = event.target.files[0];
            this.convertImage(file);
        },
        //converte o arquivo de imagem selecionado para o formato WebP
        convertImage(file) {
            const reader = new FileReader();

            reader.onload = (event) => {
                const img = new Image();

                img.onload = () => {
                    const canvas = document.createElement('canvas');
                    const ctx = canvas.getContext('2d');

                    canvas.width = img.width;
                    canvas.height = img.height;

                    ctx.drawImage(img, 0, 0);

                    canvas.toBlob((blob) => {
                        const convertedImage = new File([blob], file.name, { type: 'image/webp' });
                        this.downloadImage(convertedImage);
                    }, 'image/webp');
                };

                img.src = event.target.result;
            };

            reader.readAsDataURL(file);
        },
        //faz o download da imagem convertida
        downloadImage(image) {
            const link = document.createElement('a');
            link.href = URL.createObjectURL(image);
            link.download = 'converted_image.webp';
            link.click();
        },
        //verifica se um arquivo está selecionado e chama o método convertImage para iniciar o processo de conversão e download
        convertAndDownload() {
            const fileInput = this.$refs.fileInput;
            if (fileInput.files.length > 0) {
                const file = fileInput.files[0];
                this.convertImage(file);
            }
        }
    }
};
</script>
  