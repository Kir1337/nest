//Фильтры: Преобразования Лапласа, Собеля (картинка 255х255)
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<script type="text/javascript" src="preobr.js"></script>
</head>
<body>
<tittle>Photo</tittle>
<div>
	<input type="image" src="123.png">
	<div>
	<input type="button" id="111" onclick="GO()" value="GO">
	</div>
	<div>
	<canvas id="canvas" src="123.png" width="301px" height="301px"></canvas>
	<canvas id="canvas2" src="123.png" width="301px" height="301px"></canvas>
	<canvas id="canvas3" src="123.png" width="301px" height="301px"></canvas>
	<canvas id="canvas4" src="123.png" width="301px" height="301px"></canvas>
</div>
</div>
</div>
<div>
</div>
</body>
</html>
///////SCRYPT///////////////
function GO(){
var PowPR = function(imageData){//Степенное преобразование
var pixels2=imageData.data;
for (var i = 0; i < pixels2.length; i += 4) {
var r2 = pixels2[i];
var g2 = pixels2[i + 1];
var b2 = pixels2[i + 2];

/*pixels2[i] =Math.log (r2 +1)*30;
pixels2[i + 1] = Math.log (g2 +1)*30;
pixels2[i + 2] = Math.log (b2 +1)*30;*/
pixels2[i] =Math.pow (r2 +1,1/2)*30;
pixels2[i + 1] = Math.pow (g2 +1,1/2)*30;
pixels2[i + 2] = Math.pow (b2 +1,1/2)*30;
}
return imageData;
};

var sepia = function(imageData){//Логарифмическое преобразование
var pixels2=imageData.data;
for (var i = 0; i < pixels2.length; i += 4) {
var r2 = pixels2[i];
var g2 = pixels2[i + 1];
var b2 = pixels2[i + 2];

pixels2[i] =Math.log (r2 +1)*30;
pixels2[i + 1] = Math.log (g2 +1)*30;
pixels2[i + 2] = Math.log (b2 +1)*30;
/*pixels2[i] =Math.pow (r2 +1,1/2)*30;
pixels2[i + 1] = Math.pow (g2 +1,1/2)*30;
pixels2[i + 2] = Math.pow (b2 +1,1/2)*30;*/
}
return imageData;
};

var grayscale = function (imageData) {//Преобразование Лапласа
var pixels = imageData.data;
var Width=255*4;
var Height=255*4;
var sum={};
var count=0;
var dsum={};
var temp={};

for (var i = 0; i < pixels.length-2; i += 4) {
	sum[count]=pixels[i]+pixels[i+1]+pixels[i+2];count++
}
for (var i =254; i >=0; i --) {
	dsum[i]=[];
	for (var j =254; j >=0; j-- ) {
dsum[i][j]=sum[count--];}}
count=0;
for (var i = 1; i < 254; i += 1) {
for (var j = 1; j < 254; j += 1) {

temp[count++]= 8*dsum[i][j]-dsum[i-1][j-1]-dsum[i-1][j]-dsum[i][j-1]-dsum[i+1][j]-dsum[i][j+1]-dsum[i+1][j+1]-dsum[i-1][j+1]-dsum[i+1][j-1];}}
count=0;
for (var i = 255*4+4; i < pixels.length-255-4; i += 4){ 
	if (((i) % (255*4) <= 4 )){ continue;}
if ( temp[count++]> 100){ 
pixels[i] = 255; 
pixels[i+1] = 255; 
pixels[i+2] = 255; 
} 
else { 
pixels[i] = 0; 
pixels[i+1] = 0; 
pixels[i+2] = 0; 
} 
}
return imageData;
};


var Sobel = function (imageData) {//Преобразование Собеля

////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////	
var pixels = imageData.data;
var Width=255*4;
var Height=255*4;
var sum={};
var count=0;
var dsum={};
var temp={};

for (var i = 0; i < pixels.length-2; i += 4) {
	sum[count]=pixels[i]+pixels[i+1]+pixels[i+2];count++
}
for (var i =254; i >=0; i --) {
	dsum[i]=[];
	for (var j =254; j >=0; j-- ) {
dsum[i][j]=sum[count--];}}
count=0;
for (var i = 1; i < 254; i += 1) {
for (var j = 1; j < 254; j += 1) {




var Gx=-1*dsum[i-1][j-1]+dsum[i-1][j+1]+(-2)*dsum[i][j-1]+2*dsum[i][j+1]+(-1)*dsum[i+1][j-1]+dsum[i+1][j+1];Gx=Gx*Gx;
var Gy=-1*dsum[i-1][j-1]+(-2)*dsum[i-1][j]+(-1)*dsum[i-1][j+1]+dsum[i+1][j-1]+2*dsum[i+1][j]+dsum[i+1][j+1];Gy=Gy*Gy;
temp[count++]=Math.sqrt(Gx+Gy);}}
count=0;
for (var i = 255*4+4; i < pixels.length-255-4; i += 4){ 
	if (((i) % (255*4) <= 4 )){ continue;}
if ( temp[count++]> 100){ 
pixels[i] = 255; 
pixels[i+1] = 255; 
pixels[i+2] = 255; 
} 
else { 
pixels[i] = 0; 
pixels[i+1] = 0; 
pixels[i+2] = 0; 
} 
}
return imageData;
};
///////////////////////////////////////////


var img = new Image;
img.onload = function() {
var canvas = document.getElementById('canvas');
var context = canvas.getContext('2d');
context.drawImage(img, 0, 0);
var imageData = context.getImageData(0, 0, 255, 255);
imageDataFiltered = grayscale(imageData);
context.putImageData(imageDataFiltered, 0, 0);

var canvas2 = document.getElementById('canvas2');
var context2 = canvas2.getContext('2d');
context2.drawImage(img, 0, 0);
var imageData2 = context2.getImageData(0, 0, 300, 300);
imageDataFiltered = sepia(imageData2);
context2.putImageData(imageDataFiltered, 0, 0);

var canvas3 = document.getElementById('canvas3');
var context3 = canvas3.getContext('2d');
context3.drawImage(img, 0, 0);
var imageData3 = context3.getImageData(0, 0, 255, 255);
imageDataFiltered = Sobel(imageData3);
context3.putImageData(imageDataFiltered, 0, 0);

var canvas4= document.getElementById('canvas4');
var context4 = canvas4.getContext('2d');
context4.drawImage(img, 0, 0);
var imageData4 = context4.getImageData(0, 0, 255, 255);
imageDataFiltered = PowPR(imageData4);
context4.putImageData(imageDataFiltered, 0, 0);
};
img.src = '123.png';//Картинка 255х255
};
//////////////////////////////////////////////


