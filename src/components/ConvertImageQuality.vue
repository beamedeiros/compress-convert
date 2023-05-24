<template>
    <div>
        <input type="file" ref="fileInput" accept="image/*" @change="handleFileChange" />
        <button @click="convertAndDownload">Converter e baixar em WebP (qualidade)</button>
    </div>
</template>
  
<script>
export default {
    name: 'ImageQuality',
    methods: {
        //recebe o evento como parâmetro e extrai o arquivo selecionado do evento
        handleFileChange(event) {
            const file = event.target.files[0];
            this.convertImage(file); //passa o arquivo como argumento
        },
        //recebe um arquivo como entrada
        convertImage(file) {
            const reader = new FileReader(); //lê o conteúdo do arquivo selecionado

            //quando o arquivo é carregado, é criado um objeto Image e definido um manipulador de evento onload para a imagem
            reader.onload = (event) => {
                const img = new Image();
                
                img.onload = () => {
                    //dispara o processo de conversão chamando a função e passando a imagem carregada como argumento
                    createImageBitmap(img) 

                    //converte a imagem em um bitmap
                        .then((bitmap) => {
                            //passa o bitmap como argumento 
                            const convertedImage = this.convertToWebP(bitmap); 
                            this.downloadImage(convertedImage); 
                        })
                        .catch((error) => {
                            console.error('Error converting image:', error);
                        });
                };

                img.src = event.target.result;
            };

            reader.readAsDataURL(file);
        },
        //converte o bitmap em formato WebP e retorna uma representação de URL da imagem convertida
        convertToWebP(image) {
            const canvas = document.createElement('canvas');
            const context = canvas.getContext('2d');
            canvas.width = image.width;
            canvas.height = image.height;
            context.drawImage(image, 0, 0);
            return canvas.toDataURL('image/webp', 1.0);
        },
        //inicia o download da imagem convertida
        downloadImage(imageData) {
            const link = document.createElement('a');
            link.href = imageData;
            link.download = 'converted_image.webp';
            link.click();
        },
        //ecupera a referência do elemento de entrada de arquivo e inicia o processo de conversão da imagem
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
  