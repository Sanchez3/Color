# Color类型总结

不同的颜色系统、色彩模型、表示方法

RGB RGBA

HSL HSLA 

3-chars Hex 6-chars Hex 

X11 color names

## Function

### RGB ～ Hex

#### RGB to Hex

```javascript
function componentToHex(c) {
    var hex = c.toString(16);
    return hex.length == 1 ? "0" + hex : hex;
}

function rgbToHex(r, g, b) {
    return "#" + componentToHex(r) + componentToHex(g) + componentToHex(b);
}
console.log(rgbToHex(0, 51, 255)); //#0033ff
```

```javascript
function rgbToHex(r, g, b) {
    return "#" + ((1 << 24) + (r << 16) + (g << 8) + b).toString(16).slice(1);
}
```

#### Hex to RGB

```javascript
function hexToRgb(hex) {
    var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
    return result ? {
        r: parseInt(result[1], 16),
        g: parseInt(result[2], 16),
        b: parseInt(result[3], 16)
    } : null;
}
console.log(hexToRgb("#0033ff")); //{r: 0, g: 51, b: 255}
```

### RGB ～ HSL

#### RGB to HSL

```javascript
// r,g,b范围为[0,255],转换成h范围为[0,360]
// s,l为百分比形式，范围是[0,100],可根据需求做相应调整
function rgbtoHsl(r,g,b){
	r=r/255;
	g=g/255;
	b=b/255;
	var min=Math.min(r,g,b);
	var max=Math.max(r,g,b);
	var l=(min+max)/2;
	var difference = max-min;
	var h,s,l;
	if(max==min){
		h=0;
		s=0;
	}else{
		s=l>0.5?difference/(2.0-max-min):difference/(max+min);
		switch(max){
			case r: h=(g-b)/difference+(g < b ? 6 : 0);break;
			case g: h=2.0+(b-r)/difference;break;
			case b: h=4.0+(r-g)/difference;break;
		}
		h=Math.round(h*60);
	}
	s=Math.round(s*100);//转换成百分比的形式
	l=Math.round(l*100);
	return [h,s,l];
}
console.log(rgbtohsl(2,5,10));	//[218, 67, 2]
```

#### HSL to RGB

```javascript
//输入的h范围为[0,360],s,l为百分比形式的数值,范围是[0,100] 
//输出r,g,b范围为[0,255],可根据需求做相应调整
function hsltoRgb(h,s,l){
	var h=h/360;
	var s=s/100;
	var l=l/100;
	var rgb=[];
	if(s==0){
		rgb=[Math.round(l*255),Math.round(l*255),Math.round(l*255)];
	}else{
		var q=l>=0.5?(l+s-l*s):(l*(1+s));
		var p=2*l-q;
		var tr=rgb[0]=h+1/3;
		var tg=rgb[1]=h;
		var tb=rgb[2]=h-1/3;
		for(var i=0; i<rgb.length;i++){
			var tc=rgb[i];
			console.log(tc);
			if(tc<0){
				tc=tc+1;
			}else if(tc>1){
				tc=tc-1;
			}
			switch(true){
				case (tc<(1/6)):
					tc=p+(q-p)*6*tc;
					break;
				case ((1/6)<=tc && tc<0.5):
					tc=q;
					break;
				case (0.5<=tc && tc<(2/3)):
					tc=p+(q-p)*(4-6*tc);
					break;
				default:
					tc=p;
					break;
			}
			rgb[i]=Math.round(tc*255);
		}
	}
	return rgb;
}
console.log(hsltoRgb(90,20,30));	//[77, 92, 61]
```


## Reference

- [JS实现RGB,HSL,HSB相互转换](http://syean.cn/2017/03/17/JS%E5%AE%9E%E7%8E%B0RGB-HSL-HSB%E7%9B%B8%E4%BA%92%E8%BD%AC%E6%8D%A2/)
- [RGB to Hex and Hex to RGB](https://stackoverflow.com/questions/5623838/rgb-to-hex-and-hex-to-rgb)
- [HSL to RGB / RGB to HSL / Hex Colour Converter](https://serennu.com/colour/hsltorgb.php)

