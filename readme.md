hey

Just Some Updates
$(document).ready(function(){
  $('#download').on('click', function(){
    domtoimage.toPng(document.getElementById('appts'), {bgcolor: 'white'})
    .then(function (dataUrl) {
      var img = new Image();
      img.src = dataUrl;
      img.onload = function() {
        var canvas = document.createElement('canvas');
        var ctx = canvas.getContext('2d');
        canvas.width = img.width;
        canvas.height = img.height;
        ctx.drawImage(img, 0, 0);

        var pageWidth = 180; // set the width of each page
        var pageHeight = 250; // set the height of each page
        var pageCount = Math.ceil(canvas.height / pageHeight); // calculate the number of pages
        for (var i = 0; i < pageCount; i++) {
          var pageCanvas = document.createElement('canvas');
          var pageCtx = pageCanvas.getContext('2d');
          pageCanvas.width = canvas.width;
          pageCanvas.height = pageHeight;
          pageCtx.drawImage(canvas, 0, i * pageHeight, canvas.width, pageHeight, 0, 0, canvas.width, pageHeight);
          
          var pageImgData = pageCanvas.toDataURL('image/png');
          var pdf = new jsPDF();
          pdf.addImage(pageImgData, 'PNG', 10, 10, pageWidth, 0);
          if (i < pageCount - 1) {
            pdf.addPage(); // add a new page if there are more pages to come
          }
          pdf.save('my-timeline.pdf');
        }
      };
    });
  });
});
