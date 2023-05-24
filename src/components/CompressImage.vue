<template>
  <div>
    <input type="file" ref="fileInput" accept="image/*" @change="handleFileChange" />
    <button @click="compressAndDownload">Comprimir e baixar</button>
  </div>
</template>
  
<script>
export default {
  name: 'compressImage',
  data() {
    return {
      compressedImage: null
    };
  },
  methods: {
    //recupera o arquivo selecionado
    handleFileChange(event) {
      const file = event.target.files[0];
      this.compressImage(file);
    },
    //comprime a imagem selecionada
    compressImage(file) {
      //lê o arquivo e cria um objeto Image para carregar a imagem
      const reader = new FileReader();

      reader.onload = (event) => {
        const img = new Image();

        img.onload = () => {
          const canvas = document.createElement('canvas');
          const ctx = canvas.getContext('2d');

          // define as dimensões máximas desejadas para a imagem comprimida
          //ajusta a largura e altura da imagem original para se encaixarem dentro desses limitess
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

          //converter o canvas em um blob de imagem com qualidade configurada para 0.7 (70%)
          canvas.toBlob((blob) => {
            this.compressedImage = new File([blob], file.name, { type: 'image/jpeg' });
          }, 'image/jpeg', 0.7); //1 melhora a qualidade mas aumenta o tamanho da imagem
        };

        img.src = event.target.result;
      };

      reader.readAsDataURL(file);
    },
    //inicia o download 
    compressAndDownload() {
      if (this.compressedImage) {
        const link = document.createElement('a');
        link.href = URL.createObjectURL(this.compressedImage);
        link.download = 'compressed_image.jpg';
        link.click();
      }
    }
  }
};
</script>
  