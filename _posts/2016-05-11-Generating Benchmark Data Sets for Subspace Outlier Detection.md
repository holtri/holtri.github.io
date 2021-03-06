---
title: "Generating Benchmark Data Sets for Subspace Outlier Detection"
layout: post
---



Novel approaches for outlier detection in high dimensional data sets are typically benchmarked with real world and synthetic data sets. This blog post describes a simple approach to generate high dimensional data sets in which outliers are hidden in high-dimensional correlated subspaces. 

The data generation is implemented as an R-script that allows to configure the following parameters:


```r
datasetNumber <- '1'

numRelevantDim <- 30 # number dimensions to create correlated subspaces and place outliers
numNonRelevant <- 10 # number dimensions to create highly correlated low-dimensional subspaces without outliers

numObjects <- 1000 # total number of data objects

minSubspaceSize <- 2 # minimum dimensionality of subspaces that are created
maxSubspaceSize <- 3 # maximum dimensionality of subspaces that are created
numOutliersPerSubspace <- 4 #number of outliers that are placed in each of the relevant subspaces
intervals <- list(c(0, 0.4), c(0.6, 0.9)) # intervals to distinguish between regions of inliers and outliers

symmetric <- F # if true, the distribution of data objects in the subspace is pairwise symmetric 
```

The number of relevant dimensions to create subspaces are split into subspaces. For example, a total of 10 dimensions could be split into two 2-dim and two 3-dim subspaces. For each of the subspaces, the objects are placed by generating a random vector that fulfills the following conditions:

* For __symmetric__ subspaces: An __inlier__ lies either in none of the provided intervals or in all intervals but one. An __outlier__ lies either in all intervals or in exactly one.
* For __asymmetric__ subspaces: The placement of inlying and outlying objects is the same like in symmetric spaces but the conditions are skipped for one of the intervals for one of the attributes in the subspace.

Creating subspaces in such a manner guarantees that every outlier is not-visible in any of the lower-dimensional projections of the subspace.

If we run the script with the above configuration, we create the following subspaces



```r
set.seed(42)
generated <- generateDataSet(datasetNumber, numRelevantDim, numNonRelevant, 
                              numObjects, minSubspaceSize, maxSubspaceSize, 
                              numOutliersPerSubspace, intervals, symmetric)
```

```
## [1] 1 2
## [1] 3 4
## [1] 5 6
## [1] 7 8
## [1]  9 10
## [1] 11 12
## [1] 13 14
## [1] 15 16 17
## [1] 18 19 20
## [1] 21 22 23
## [1] 24 25 26
## [1] 27 28 29 30
## [1] "number of generated outliers:  46"
## [1] "number of generated subspaces:  12"
```

Two example plots for a two-dimensional space and three dimensional space (click on graphic to rotate) for the asymmetric case.

![plot of chunk plot2d](../images/blogpost-plot2d-1.png)

In the two-dimensional plot above there are four objects visible as outliers, the other outliers are hidden in this space. Also note that both one-dimensional projections cause the outliers that are visible in the subspace to disappear.


```r
plotSubspace(generated$data, generated$subspaces[[8]])
```

<script>CanvasMatrix4=function(m){if(typeof m=='object'){if("length"in m&&m.length>=16){this.load(m[0],m[1],m[2],m[3],m[4],m[5],m[6],m[7],m[8],m[9],m[10],m[11],m[12],m[13],m[14],m[15]);return}else if(m instanceof CanvasMatrix4){this.load(m);return}}this.makeIdentity()};CanvasMatrix4.prototype.load=function(){if(arguments.length==1&&typeof arguments[0]=='object'){var matrix=arguments[0];if("length"in matrix&&matrix.length==16){this.m11=matrix[0];this.m12=matrix[1];this.m13=matrix[2];this.m14=matrix[3];this.m21=matrix[4];this.m22=matrix[5];this.m23=matrix[6];this.m24=matrix[7];this.m31=matrix[8];this.m32=matrix[9];this.m33=matrix[10];this.m34=matrix[11];this.m41=matrix[12];this.m42=matrix[13];this.m43=matrix[14];this.m44=matrix[15];return}if(arguments[0]instanceof CanvasMatrix4){this.m11=matrix.m11;this.m12=matrix.m12;this.m13=matrix.m13;this.m14=matrix.m14;this.m21=matrix.m21;this.m22=matrix.m22;this.m23=matrix.m23;this.m24=matrix.m24;this.m31=matrix.m31;this.m32=matrix.m32;this.m33=matrix.m33;this.m34=matrix.m34;this.m41=matrix.m41;this.m42=matrix.m42;this.m43=matrix.m43;this.m44=matrix.m44;return}}this.makeIdentity()};CanvasMatrix4.prototype.getAsArray=function(){return[this.m11,this.m12,this.m13,this.m14,this.m21,this.m22,this.m23,this.m24,this.m31,this.m32,this.m33,this.m34,this.m41,this.m42,this.m43,this.m44]};CanvasMatrix4.prototype.getAsWebGLFloatArray=function(){return new WebGLFloatArray(this.getAsArray())};CanvasMatrix4.prototype.makeIdentity=function(){this.m11=1;this.m12=0;this.m13=0;this.m14=0;this.m21=0;this.m22=1;this.m23=0;this.m24=0;this.m31=0;this.m32=0;this.m33=1;this.m34=0;this.m41=0;this.m42=0;this.m43=0;this.m44=1};CanvasMatrix4.prototype.transpose=function(){var tmp=this.m12;this.m12=this.m21;this.m21=tmp;tmp=this.m13;this.m13=this.m31;this.m31=tmp;tmp=this.m14;this.m14=this.m41;this.m41=tmp;tmp=this.m23;this.m23=this.m32;this.m32=tmp;tmp=this.m24;this.m24=this.m42;this.m42=tmp;tmp=this.m34;this.m34=this.m43;this.m43=tmp};CanvasMatrix4.prototype.invert=function(){var det=this._determinant4x4();if(Math.abs(det)<1e-8)return null;this._makeAdjoint();this.m11/=det;this.m12/=det;this.m13/=det;this.m14/=det;this.m21/=det;this.m22/=det;this.m23/=det;this.m24/=det;this.m31/=det;this.m32/=det;this.m33/=det;this.m34/=det;this.m41/=det;this.m42/=det;this.m43/=det;this.m44/=det};CanvasMatrix4.prototype.translate=function(x,y,z){if(x==undefined)x=0;if(y==undefined)y=0;if(z==undefined)z=0;var matrix=new CanvasMatrix4();matrix.m41=x;matrix.m42=y;matrix.m43=z;this.multRight(matrix)};CanvasMatrix4.prototype.scale=function(x,y,z){if(x==undefined)x=1;if(z==undefined){if(y==undefined){y=x;z=x}else z=1}else if(y==undefined)y=x;var matrix=new CanvasMatrix4();matrix.m11=x;matrix.m22=y;matrix.m33=z;this.multRight(matrix)};CanvasMatrix4.prototype.rotate=function(angle,x,y,z){angle=angle/180*Math.PI;angle/=2;var sinA=Math.sin(angle);var cosA=Math.cos(angle);var sinA2=sinA*sinA;var length=Math.sqrt(x*x+y*y+z*z);if(length==0){x=0;y=0;z=1}else if(length!=1){x/=length;y/=length;z/=length}var mat=new CanvasMatrix4();if(x==1&&y==0&&z==0){mat.m11=1;mat.m12=0;mat.m13=0;mat.m21=0;mat.m22=1-2*sinA2;mat.m23=2*sinA*cosA;mat.m31=0;mat.m32=-2*sinA*cosA;mat.m33=1-2*sinA2;mat.m14=mat.m24=mat.m34=0;mat.m41=mat.m42=mat.m43=0;mat.m44=1}else if(x==0&&y==1&&z==0){mat.m11=1-2*sinA2;mat.m12=0;mat.m13=-2*sinA*cosA;mat.m21=0;mat.m22=1;mat.m23=0;mat.m31=2*sinA*cosA;mat.m32=0;mat.m33=1-2*sinA2;mat.m14=mat.m24=mat.m34=0;mat.m41=mat.m42=mat.m43=0;mat.m44=1}else if(x==0&&y==0&&z==1){mat.m11=1-2*sinA2;mat.m12=2*sinA*cosA;mat.m13=0;mat.m21=-2*sinA*cosA;mat.m22=1-2*sinA2;mat.m23=0;mat.m31=0;mat.m32=0;mat.m33=1;mat.m14=mat.m24=mat.m34=0;mat.m41=mat.m42=mat.m43=0;mat.m44=1}else{var x2=x*x;var y2=y*y;var z2=z*z;mat.m11=1-2*(y2+z2)*sinA2;mat.m12=2*(x*y*sinA2+z*sinA*cosA);mat.m13=2*(x*z*sinA2-y*sinA*cosA);mat.m21=2*(y*x*sinA2-z*sinA*cosA);mat.m22=1-2*(z2+x2)*sinA2;mat.m23=2*(y*z*sinA2+x*sinA*cosA);mat.m31=2*(z*x*sinA2+y*sinA*cosA);mat.m32=2*(z*y*sinA2-x*sinA*cosA);mat.m33=1-2*(x2+y2)*sinA2;mat.m14=mat.m24=mat.m34=0;mat.m41=mat.m42=mat.m43=0;mat.m44=1}this.multRight(mat)};CanvasMatrix4.prototype.multRight=function(mat){var m11=(this.m11*mat.m11+this.m12*mat.m21+this.m13*mat.m31+this.m14*mat.m41);var m12=(this.m11*mat.m12+this.m12*mat.m22+this.m13*mat.m32+this.m14*mat.m42);var m13=(this.m11*mat.m13+this.m12*mat.m23+this.m13*mat.m33+this.m14*mat.m43);var m14=(this.m11*mat.m14+this.m12*mat.m24+this.m13*mat.m34+this.m14*mat.m44);var m21=(this.m21*mat.m11+this.m22*mat.m21+this.m23*mat.m31+this.m24*mat.m41);var m22=(this.m21*mat.m12+this.m22*mat.m22+this.m23*mat.m32+this.m24*mat.m42);var m23=(this.m21*mat.m13+this.m22*mat.m23+this.m23*mat.m33+this.m24*mat.m43);var m24=(this.m21*mat.m14+this.m22*mat.m24+this.m23*mat.m34+this.m24*mat.m44);var m31=(this.m31*mat.m11+this.m32*mat.m21+this.m33*mat.m31+this.m34*mat.m41);var m32=(this.m31*mat.m12+this.m32*mat.m22+this.m33*mat.m32+this.m34*mat.m42);var m33=(this.m31*mat.m13+this.m32*mat.m23+this.m33*mat.m33+this.m34*mat.m43);var m34=(this.m31*mat.m14+this.m32*mat.m24+this.m33*mat.m34+this.m34*mat.m44);var m41=(this.m41*mat.m11+this.m42*mat.m21+this.m43*mat.m31+this.m44*mat.m41);var m42=(this.m41*mat.m12+this.m42*mat.m22+this.m43*mat.m32+this.m44*mat.m42);var m43=(this.m41*mat.m13+this.m42*mat.m23+this.m43*mat.m33+this.m44*mat.m43);var m44=(this.m41*mat.m14+this.m42*mat.m24+this.m43*mat.m34+this.m44*mat.m44);this.m11=m11;this.m12=m12;this.m13=m13;this.m14=m14;this.m21=m21;this.m22=m22;this.m23=m23;this.m24=m24;this.m31=m31;this.m32=m32;this.m33=m33;this.m34=m34;this.m41=m41;this.m42=m42;this.m43=m43;this.m44=m44};CanvasMatrix4.prototype.multLeft=function(mat){var m11=(mat.m11*this.m11+mat.m12*this.m21+mat.m13*this.m31+mat.m14*this.m41);var m12=(mat.m11*this.m12+mat.m12*this.m22+mat.m13*this.m32+mat.m14*this.m42);var m13=(mat.m11*this.m13+mat.m12*this.m23+mat.m13*this.m33+mat.m14*this.m43);var m14=(mat.m11*this.m14+mat.m12*this.m24+mat.m13*this.m34+mat.m14*this.m44);var m21=(mat.m21*this.m11+mat.m22*this.m21+mat.m23*this.m31+mat.m24*this.m41);var m22=(mat.m21*this.m12+mat.m22*this.m22+mat.m23*this.m32+mat.m24*this.m42);var m23=(mat.m21*this.m13+mat.m22*this.m23+mat.m23*this.m33+mat.m24*this.m43);var m24=(mat.m21*this.m14+mat.m22*this.m24+mat.m23*this.m34+mat.m24*this.m44);var m31=(mat.m31*this.m11+mat.m32*this.m21+mat.m33*this.m31+mat.m34*this.m41);var m32=(mat.m31*this.m12+mat.m32*this.m22+mat.m33*this.m32+mat.m34*this.m42);var m33=(mat.m31*this.m13+mat.m32*this.m23+mat.m33*this.m33+mat.m34*this.m43);var m34=(mat.m31*this.m14+mat.m32*this.m24+mat.m33*this.m34+mat.m34*this.m44);var m41=(mat.m41*this.m11+mat.m42*this.m21+mat.m43*this.m31+mat.m44*this.m41);var m42=(mat.m41*this.m12+mat.m42*this.m22+mat.m43*this.m32+mat.m44*this.m42);var m43=(mat.m41*this.m13+mat.m42*this.m23+mat.m43*this.m33+mat.m44*this.m43);var m44=(mat.m41*this.m14+mat.m42*this.m24+mat.m43*this.m34+mat.m44*this.m44);this.m11=m11;this.m12=m12;this.m13=m13;this.m14=m14;this.m21=m21;this.m22=m22;this.m23=m23;this.m24=m24;this.m31=m31;this.m32=m32;this.m33=m33;this.m34=m34;this.m41=m41;this.m42=m42;this.m43=m43;this.m44=m44};CanvasMatrix4.prototype.ortho=function(left,right,bottom,top,near,far){var tx=(left+right)/(left-right);var ty=(top+bottom)/(top-bottom);var tz=(far+near)/(far-near);var matrix=new CanvasMatrix4();matrix.m11=2/(left-right);matrix.m12=0;matrix.m13=0;matrix.m14=0;matrix.m21=0;matrix.m22=2/(top-bottom);matrix.m23=0;matrix.m24=0;matrix.m31=0;matrix.m32=0;matrix.m33=-2/(far-near);matrix.m34=0;matrix.m41=tx;matrix.m42=ty;matrix.m43=tz;matrix.m44=1;this.multRight(matrix)};CanvasMatrix4.prototype.frustum=function(left,right,bottom,top,near,far){var matrix=new CanvasMatrix4();var A=(right+left)/(right-left);var B=(top+bottom)/(top-bottom);var C=-(far+near)/(far-near);var D=-(2*far*near)/(far-near);matrix.m11=(2*near)/(right-left);matrix.m12=0;matrix.m13=0;matrix.m14=0;matrix.m21=0;matrix.m22=2*near/(top-bottom);matrix.m23=0;matrix.m24=0;matrix.m31=A;matrix.m32=B;matrix.m33=C;matrix.m34=-1;matrix.m41=0;matrix.m42=0;matrix.m43=D;matrix.m44=0;this.multRight(matrix)};CanvasMatrix4.prototype.perspective=function(fovy,aspect,zNear,zFar){var top=Math.tan(fovy*Math.PI/360)*zNear;var bottom=-top;var left=aspect*bottom;var right=aspect*top;this.frustum(left,right,bottom,top,zNear,zFar)};CanvasMatrix4.prototype.lookat=function(eyex,eyey,eyez,centerx,centery,centerz,upx,upy,upz){var matrix=new CanvasMatrix4();var zx=eyex-centerx;var zy=eyey-centery;var zz=eyez-centerz;var mag=Math.sqrt(zx*zx+zy*zy+zz*zz);if(mag){zx/=mag;zy/=mag;zz/=mag}var yx=upx;var yy=upy;var yz=upz;xx=yy*zz-yz*zy;xy=-yx*zz+yz*zx;xz=yx*zy-yy*zx;yx=zy*xz-zz*xy;yy=-zx*xz+zz*xx;yx=zx*xy-zy*xx;mag=Math.sqrt(xx*xx+xy*xy+xz*xz);if(mag){xx/=mag;xy/=mag;xz/=mag}mag=Math.sqrt(yx*yx+yy*yy+yz*yz);if(mag){yx/=mag;yy/=mag;yz/=mag}matrix.m11=xx;matrix.m12=xy;matrix.m13=xz;matrix.m14=0;matrix.m21=yx;matrix.m22=yy;matrix.m23=yz;matrix.m24=0;matrix.m31=zx;matrix.m32=zy;matrix.m33=zz;matrix.m34=0;matrix.m41=0;matrix.m42=0;matrix.m43=0;matrix.m44=1;matrix.translate(-eyex,-eyey,-eyez);this.multRight(matrix)};CanvasMatrix4.prototype._determinant2x2=function(a,b,c,d){return a*d-b*c};CanvasMatrix4.prototype._determinant3x3=function(a1,a2,a3,b1,b2,b3,c1,c2,c3){return a1*this._determinant2x2(b2,b3,c2,c3)-b1*this._determinant2x2(a2,a3,c2,c3)+c1*this._determinant2x2(a2,a3,b2,b3)};CanvasMatrix4.prototype._determinant4x4=function(){var a1=this.m11;var b1=this.m12;var c1=this.m13;var d1=this.m14;var a2=this.m21;var b2=this.m22;var c2=this.m23;var d2=this.m24;var a3=this.m31;var b3=this.m32;var c3=this.m33;var d3=this.m34;var a4=this.m41;var b4=this.m42;var c4=this.m43;var d4=this.m44;return a1*this._determinant3x3(b2,b3,b4,c2,c3,c4,d2,d3,d4)-b1*this._determinant3x3(a2,a3,a4,c2,c3,c4,d2,d3,d4)+c1*this._determinant3x3(a2,a3,a4,b2,b3,b4,d2,d3,d4)-d1*this._determinant3x3(a2,a3,a4,b2,b3,b4,c2,c3,c4)};CanvasMatrix4.prototype._makeAdjoint=function(){var a1=this.m11;var b1=this.m12;var c1=this.m13;var d1=this.m14;var a2=this.m21;var b2=this.m22;var c2=this.m23;var d2=this.m24;var a3=this.m31;var b3=this.m32;var c3=this.m33;var d3=this.m34;var a4=this.m41;var b4=this.m42;var c4=this.m43;var d4=this.m44;this.m11=this._determinant3x3(b2,b3,b4,c2,c3,c4,d2,d3,d4);this.m21=-this._determinant3x3(a2,a3,a4,c2,c3,c4,d2,d3,d4);this.m31=this._determinant3x3(a2,a3,a4,b2,b3,b4,d2,d3,d4);this.m41=-this._determinant3x3(a2,a3,a4,b2,b3,b4,c2,c3,c4);this.m12=-this._determinant3x3(b1,b3,b4,c1,c3,c4,d1,d3,d4);this.m22=this._determinant3x3(a1,a3,a4,c1,c3,c4,d1,d3,d4);this.m32=-this._determinant3x3(a1,a3,a4,b1,b3,b4,d1,d3,d4);this.m42=this._determinant3x3(a1,a3,a4,b1,b3,b4,c1,c3,c4);this.m13=this._determinant3x3(b1,b2,b4,c1,c2,c4,d1,d2,d4);this.m23=-this._determinant3x3(a1,a2,a4,c1,c2,c4,d1,d2,d4);this.m33=this._determinant3x3(a1,a2,a4,b1,b2,b4,d1,d2,d4);this.m43=-this._determinant3x3(a1,a2,a4,b1,b2,b4,c1,c2,c4);this.m14=-this._determinant3x3(b1,b2,b3,c1,c2,c3,d1,d2,d3);this.m24=this._determinant3x3(a1,a2,a3,c1,c2,c3,d1,d2,d3);this.m34=-this._determinant3x3(a1,a2,a3,b1,b2,b3,d1,d2,d3);this.m44=this._determinant3x3(a1,a2,a3,b1,b2,b3,c1,c2,c3)}</script>
<script>
rglwidgetClass = function() {
this.canvas = null;
this.userMatrix = new CanvasMatrix4();
this.types = [];
this.prMatrix = new CanvasMatrix4();
this.mvMatrix = new CanvasMatrix4();
this.vp = null;
this.prmvMatrix = null;
this.origs = null;
this.gl = null;
this.scene = null;
};
(function() {
this.multMV = function(M, v) {
return [ M.m11 * v[0] + M.m12 * v[1] + M.m13 * v[2] + M.m14 * v[3],
M.m21 * v[0] + M.m22 * v[1] + M.m23 * v[2] + M.m24 * v[3],
M.m31 * v[0] + M.m32 * v[1] + M.m33 * v[2] + M.m34 * v[3],
M.m41 * v[0] + M.m42 * v[1] + M.m43 * v[2] + M.m44 * v[3]
];
};
this.vlen = function(v) {
return Math.sqrt(this.dotprod(v, v));
};
this.dotprod = function(a, b) {
return a[0]*b[0] + a[1]*b[1] + a[2]*b[2];
}
this.xprod = function(a, b) {
return [a[1]*b[2] - a[2]*b[1],
a[2]*b[0] - a[0]*b[2],
a[0]*b[1] - a[1]*b[0]];
};
this.cbind = function(a, b) {
return a.map(function(currentValue, index, array) {
return currentValue.concat(b[index]);
});
};
this.swap = function(a, i, j) {
var temp = a[i];
a[i] = a[j];
a[j] = temp;
};
this.flatten = function(a) {
return [].concat.apply([], a);
};
/* set element of 1d or 2d array as if it was flattened.  Column major, zero based! */
this.setElement = function(a, i, value) {
if (Array.isArray(a[0])) {
var dim = a.length,
col = Math.floor(i/dim),
row = i % dim;
a[row][col] = value;
} else {
a[i] = value;
}
};
this.transpose = function(a) {
var newArray = [],
n = a.length,
m = a[0].length,
i;
for(i = 0; i < m; i++){
newArray.push([]);
}
for(i = 0; i < n; i++){
for(var j = 0; j < m; j++){
newArray[j].push(a[i][j]);
}
}
return newArray;
};
this.sumsq = function(x) {
var result = 0, i;
for (i=0; i < x.length; i++)
result += x[i]*x[i];
return result;
};
this.toCanvasMatrix4 = function(mat) {
if (mat instanceof CanvasMatrix4)
return mat;
var result = new CanvasMatrix4();
mat = this.flatten(this.transpose(mat));
result.load(mat);
return result;
};
this.stringToRgb = function(s) {
s = s.replace("#", "");
var bigint = parseInt(s, 16);
return [((bigint >> 16) & 255)/255,
((bigint >> 8) & 255)/255,
(bigint & 255)/255];
};
this.componentProduct = function(x, y) {
if (typeof y === "undefined") {
this.alertOnce("Bad arg to componentProduct");
}
var result = new Float32Array(3), i;
for (i = 0; i<3; i++)
result[i] = x[i]*y[i];
return result;
};
this.getPowerOfTwo = function(value) {
var pow = 1;
while(pow<value) {
pow *= 2;
}
return pow;
};
this.unique = function(arr) {
arr = [].concat(arr);
return arr.filter(function(value, index, self) {
return self.indexOf(value) === index;
});
};
this.repeatToLen = function(arr, len) {
arr = [].concat(arr);
while (arr.length < len/2)
arr = arr.concat(arr);
return arr.concat(arr.slice(0, len - arr.length));
};
this.alertOnce = function(msg) {
if (typeof this.alerted !== "undefined")
return;
this.alerted = true;
alert(msg);
};
this.f_is_lit = 1;
this.f_is_smooth = 2;
this.f_has_texture = 4;
this.f_is_indexed = 8;
this.f_depth_sort = 16;
this.f_fixed_quads = 32;
this.f_is_transparent = 64;
this.f_is_lines = 128;
this.f_sprites_3d = 256;
this.f_sprite_3d = 512;
this.f_is_subscene = 1024;
this.f_is_clipplanes = 2048;
this.whichList = function(id) {
var obj = this.getObj(id),
flags = obj.flags;
if (obj.type === "light")
return "lights";
if (flags & this.f_is_subscene)
return "subscenes";
if (flags & this.f_is_clipplanes)
return "clipplanes";
if (flags & this.f_is_transparent)
return "transparent";
return "opaque";
};
this.getObj = function(id) {
if (typeof id !== "number") {
this.alertOnce("getObj id is "+typeof id);
}
return this.scene.objects[id];
};
this.getIdsByType = function(type, subscene) {
var
result = [], i, self = this;
if (typeof subscene === "undefined") {
Object.keys(this.scene.objects).forEach(
function(key) {
key = parseInt(key, 10);
if (self.getObj(key).type === type)
result.push(key);
});
} else {
ids = this.getObj(subscene).objects;
for (i=0; i < ids.length; i++) {
if (this.getObj(ids[i]).type === type) {
result.push(ids[i]);
}
}
}
return result;
};
this.getMaterial = function(id, property) {
var obj = this.getObj(id),
mat = obj.material[property];
if (typeof mat === "undefined")
mat = this.scene.material[property];
return mat;
};
this.inSubscene = function(id, subscene) {
return this.getObj(subscene).objects.indexOf(id) > -1;
};
this.addToSubscene = function(id, subscene) {
var thelist,
thesub = this.getObj(subscene),
ids = [id],
obj = this.getObj(id), i;
if (typeof obj.newIds !== "undefined") {
ids = ids.concat(obj.newIds);
}
for (i = 0; i < ids.length; i++) {
id = ids[i];
if (thesub.objects.indexOf(id) == -1) {
thelist = this.whichList(id);
thesub.objects.push(id);
thesub[thelist].push(id);
}
}
};
this.delFromSubscene = function(id, subscene) {
var thelist,
thesub = this.getObj(subscene),
obj = this.getObj(id),
ids = [id], i, newIds;
if (typeof obj.newIds !== "undefined")
ids = ids.concat(obj.newIds);
for (j=0; j<ids.length;j++) {
id = ids[j];
i = thesub.objects.indexOf(id);
if (i > -1) {
thesub.objects.splice(i, 1);
thelist = this.whichList(id);
i = thesub[thelist].indexOf(id);
thesub[thelist].splice(i, 1);
}
}
};
this.setSubsceneEntries = function(ids, subsceneid) {
var sub = this.getObj(subsceneid);
sub.objects = ids;
this.initSubscene(subsceneid);
};
this.getSubsceneEntries = function(subscene) {
return this.getObj(subscene).objects;
};
this.getChildSubscenes = function(subscene) {
return this.getObj(subscene).subscenes;
};
this.getVertexShader = function(id) {
var obj = this.getObj(id),
flags = obj.flags,
type = obj.type,
is_lit = flags & this.f_is_lit,
has_texture = flags & this.f_has_texture,
fixed_quads = flags & this.f_fixed_quads,
sprites_3d = flags & this.f_sprites_3d,
sprite_3d = flags & this.f_sprite_3d,
nclipplanes = this.countClipplanes(),
result;
if (type === "clipplanes" || sprites_3d) return;
result = "	/* ****** "+type+" object "+id+" vertex shader ****** */\n"+
"	attribute vec3 aPos;\n"+
"	attribute vec4 aCol;\n"+
" uniform mat4 mvMatrix;\n"+
" uniform mat4 prMatrix;\n"+
" varying vec4 vCol;\n"+
" varying vec4 vPosition;\n";
if (is_lit && !fixed_quads)
result = result + "	attribute vec3 aNorm;\n"+
" uniform mat4 normMatrix;\n"+
" varying vec3 vNormal;\n";
if (has_texture || type === "text")
result = result + " attribute vec2 aTexcoord;\n"+
" varying vec2 vTexcoord;\n";
if (type === "text")
result = result + "	uniform vec2 textScale;\n";
if (fixed_quads)
result = result + "	attribute vec2 aOfs;\n";
else if (sprite_3d)
result = result + "	uniform vec3 uOrig;\n"+
" uniform float uSize;\n"+
" uniform mat4 usermat;\n";
result = result + "	void main(void) {\n";
if (nclipplanes || (!fixed_quads && !sprite_3d))
result = result + "	  vPosition = mvMatrix * vec4(aPos, 1.);\n";
if (!fixed_quads && !sprite_3d)
result = result + "	  gl_Position = prMatrix * vPosition;\n";
if (type == "points") {
var size = this.getMaterial(id, "size");
result = result + "	  gl_PointSize = "+size.toFixed(1)+";\n";
}
result = result + "	  vCol = aCol;\n";
if (is_lit && !fixed_quads && !sprite_3d)
result = result + "	  vNormal = normalize((normMatrix * vec4(aNorm, 1.)).xyz);\n";
if (has_texture || type === "text")
result = result + "	  vTexcoord = aTexcoord;\n";
if (type == "text")
result = result + "	  vec4 pos = prMatrix * mvMatrix * vec4(aPos, 1.);\n"+
"   pos = pos/pos.w;\n"+
"   gl_Position = pos + vec4(aOfs*textScale, 0.,0.);\n";
if (type == "sprites")
result = result + "	  vec4 pos = mvMatrix * vec4(aPos, 1.);\n"+
"   pos = pos/pos.w + vec4(aOfs, 0., 0.);\n"+
"   gl_Position = prMatrix*pos;\n";
if (sprite_3d)
result = result + "	  vNormal = normalize((normMatrix * vec4(aNorm, 1.)).xyz);\n"+
"   vec4 pos = mvMatrix * vec4(uOrig, 1.);\n"+
"   vPosition = pos/pos.w + vec4(uSize*(vec4(aPos, 1.)*usermat).xyz,0.);\n"+
"   gl_Position = prMatrix * vPosition;\n";
result = result + "	}\n";
return result;
};
this.getFragmentShader = function(id) {
var obj = this.getObj(id),
flags = obj.flags,
type = obj.type,
is_lit = flags & this.f_is_lit,
has_texture = flags & this.f_has_texture,
fixed_quads = flags & this.f_fixed_quads,
sprites_3d = flags & this.f_sprites_3d,
nclipplanes = this.countClipplanes(), i,
texture_format, nlights,
result;
if (type === "clipplanes" || sprites_3d) return;
if (has_texture)
texture_format = this.getMaterial(id, "textype");
result = "/* ****** "+type+" object "+id+" fragment shader ****** */\n"+
"#ifdef GL_ES\n"+
"  precision highp float;\n"+
"#endif\n"+
"  varying vec4 vCol; // carries alpha\n"+
"  varying vec4 vPosition;\n";
if (has_texture || type === "text")
result = result + "	varying vec2 vTexcoord;\n"+
" uniform sampler2D uSampler;\n";
if (is_lit && !fixed_quads)
result = result + "	varying vec3 vNormal;\n";
for (i = 0; i < nclipplanes; i++)
result = result + "	uniform vec4 vClipplane"+i+";\n";
if (is_lit) {
nlights = this.countLights();
if (nlights)
result = result + "	uniform mat4 mvMatrix;\n";
else
is_lit = false;
}
if (is_lit) {
result = result + "	  uniform vec3 emission;\n"+
"   uniform float shininess;\n";
for (i=0; i < nlights; i++) {
result = result + "	  uniform vec3 ambient" + i + ";\n"+
"   uniform vec3 specular" + i +"; // light*material\n"+
"   uniform vec3 diffuse" + i + ";\n"+
"   uniform vec3 lightDir" + i + ";\n"+
"   uniform bool viewpoint" + i + ";\n"+
"   uniform bool finite" + i + ";\n";
}
}
result = result + "	void main(void) {\n";
for (i=0; i < nclipplanes;i++)
result = result + "	  if (dot(vPosition, vClipplane"+i+") < 0.0) discard;\n";
if (is_lit) {
result = result + "	  vec3 eye = normalize(-vPosition.xyz);\n"+
"   vec3 lightdir;\n"+
"   vec4 colDiff;\n"+
"   vec3 halfVec;\n"+
"   vec4 lighteffect = vec4(emission, 0.);\n"+
"   vec3 col;\n"+
"   float nDotL;\n";
if (fixed_quads) {
result = result +   "	  vec3 n = vec3(0., 0., 1.);\n";
}
else {
result = result +   "	  vec3 n = normalize(vNormal);\n"+
"   n = -faceforward(n, n, eye);\n";
}
for (i=0; i < nlights; i++) {
result = result + "   colDiff = vec4(vCol.rgb * diffuse" + i + ", vCol.a);\n"+
"   lightdir = lightDir" + i + ";\n"+
"   if (!viewpoint" + i +")\n"+
"     lightdir = (mvMatrix * vec4(lightdir, 1.)).xyz;\n"+
"   if (!finite" + i + ") {\n"+
"     halfVec = normalize(lightdir + eye);\n"+
"   } else {\n"+
"     lightdir = normalize(lightdir - vPosition.xyz);\n"+
"     halfVec = normalize(lightdir + eye);\n"+
"   }\n"+
"	  col = ambient" + i + ";\n"+
"   nDotL = dot(n, lightdir);\n"+
"   col = col + max(nDotL, 0.) * colDiff.rgb;\n"+
"   col = col + pow(max(dot(halfVec, n), 0.), shininess) * specular" + i + ";\n"+
"   lighteffect = lighteffect + vec4(col, colDiff.a);\n";
}
} else {
result = result +   "   vec4 colDiff = vCol;\n"+
"	  vec4 lighteffect = colDiff;\n";
}
if ((has_texture && texture_format === "rgba") || type === "text")
result = result +   "	  vec4 textureColor = lighteffect*texture2D(uSampler, vTexcoord);\n";
if (has_texture) {
result = result + {
rgb:            "   vec4 textureColor = lighteffect*vec4(texture2D(uSampler, vTexcoord).rgb, 1.);\n",
alpha:          "   vec4 textureColor = texture2D(uSampler, vTexcoord);\n"+
"   float luminance = dot(vec3(1.,1.,1.), textureColor.rgb)/3.;\n"+
"   textureColor =  vec4(lighteffect.rgb, lighteffect.a*luminance);\n",
luminance:      "   vec4 textureColor = vec4(lighteffect.rgb*dot(texture2D(uSampler, vTexcoord).rgb, vec3(1.,1.,1.))/3., lighteffect.a);\n",
"luminance.alpha":"	  vec4 textureColor = texture2D(uSampler, vTexcoord);\n"+
"   float luminance = dot(vec3(1.,1.,1.),textureColor.rgb)/3.;\n"+
"   textureColor = vec4(lighteffect.rgb*luminance, lighteffect.a*textureColor.a);\n"
}[texture_format]+
"   gl_FragColor = textureColor;\n";
} else if (type === "text") {
result = result +   "	  if (textureColor.a < 0.1)\n"+
"     discard;\n"+
"   else\n"+
"     gl_FragColor = textureColor;\n";
} else
result = result +   "   gl_FragColor = lighteffect;\n";
result = result + "	}\n";
return result;
};
this.getShader = function(shaderType, code) {
var gl = this.gl, shader;
shader = gl.createShader(shaderType);
gl.shaderSource(shader, code);
gl.compileShader(shader);
if (gl.getShaderParameter(shader, gl.COMPILE_STATUS) === 0)
alert(gl.getShaderInfoLog(shader));
return shader;
};
this.handleLoadedTexture = function(texture, textureCanvas) {
var gl = this.gl;
gl.pixelStorei(gl.UNPACK_FLIP_Y_WEBGL, true);
gl.bindTexture(gl.TEXTURE_2D, texture);
gl.texImage2D(gl.TEXTURE_2D, 0, gl.RGBA, gl.RGBA, gl.UNSIGNED_BYTE, textureCanvas);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MAG_FILTER, gl.LINEAR);
gl.texParameteri(gl.TEXTURE_2D, gl.TEXTURE_MIN_FILTER, gl.LINEAR_MIPMAP_NEAREST);
gl.generateMipmap(gl.TEXTURE_2D);
gl.bindTexture(gl.TEXTURE_2D, null);
};
this.loadImageToTexture = function(uri, texture) {
var canvas = this.textureCanvas,
ctx = canvas.getContext("2d"),
image = new Image(),
self = this;
image.onload = function() {
var w = image.width,
h = image.height,
canvasX = self.getPowerOfTwo(w),
canvasY = self.getPowerOfTwo(h),
gl = self.gl,
maxTexSize = gl.getParameter(gl.MAX_TEXTURE_SIZE);
if (maxTexSize > 4096) maxTexSize = 4096;
while (canvasX > 1 && canvasY > 1 && (canvasX > maxTexSize || canvasY > maxTexSize)) {
canvasX /= 2;
canvasY /= 2;
}
canvas.width = canvasX;
canvas.height = canvasY;
ctx.imageSmoothingEnabled = true;
ctx.drawImage(image, 0, 0, canvasX, canvasY);
self.handleLoadedTexture(texture, canvas);
self.drawScene();
};
image.src = uri;
};
this.drawTextToCanvas = function(text, cex, family, font) {
var canvasX, canvasY,
textY,
scaling = 20,
textColour = "white",
backgroundColour = "rgba(0,0,0,0)",
canvas = this.textureCanvas,
ctx = canvas.getContext("2d"),
i, textHeights = [], widths = [], offset = 0, offsets = [],
fontStrings = [],
getFontString = function(i) {
textHeights[i] = scaling*cex[i];
var fontString = textHeights[i] + "px",
family0 = family[i],
font0 = font[i];
if (family0 === "sans")
family0 = "sans-serif";
else if (family0 === "mono")
family0 = "monospace";
fontString = fontString + " " + family0;
if (font0 === 2 || font0 === 4)
fontString = "bold " + fontString;
if (font0 === 3 || font0 === 4)
fontString = "italic " + fontString;
return fontString;
};
cex = this.repeatToLen(cex, text.length);
family = this.repeatToLen(family, text.length);
font = this.repeatToLen(font, text.length);
canvasX = 1;
for (i = 0; i < text.length; i++)  {
ctx.font = fontStrings[i] = getFontString(i);
widths[i] = ctx.measureText(text[i]).width;
offset = offsets[i] = offset + 2*textHeights[i];
canvasX = (widths[i] > canvasX) ? widths[i] : canvasX;
}
canvasX = this.getPowerOfTwo(canvasX);
canvasY = this.getPowerOfTwo(offset);
canvas.width = canvasX;
canvas.height = canvasY;
ctx.fillStyle = backgroundColour;
ctx.fillRect(0, 0, ctx.canvas.width, ctx.canvas.height);
ctx.textBaseline = "alphabetic";
for(i = 0; i < text.length; i++) {
textY = offsets[i];
ctx.font = fontStrings[i];
ctx.fillStyle = textColour;
ctx.textAlign = "left";
ctx.fillText(text[i], 0,  textY);
}
return {canvasX:canvasX, canvasY:canvasY,
widths:widths, textHeights:textHeights,
offsets:offsets};
};
this.setViewport = function(id) {
var gl = this.gl,
vp = this.getObj(id).par3d.viewport,
x = vp.x*this.canvas.width,
y = vp.y*this.canvas.height,
width = vp.width*this.canvas.width,
height = vp.height*this.canvas.height;
this.vp = {x:x, y:y, width:width, height:height};
gl.viewport(x, y, width, height);
gl.scissor(x, y, width, height);
};
this.setprMatrix = function(id) {
var subscene = this.getObj(id),
embedding = subscene.embeddings.projection;
if (embedding === "replace")
this.prMatrix.makeIdentity();
else
this.setprMatrix(subscene.parent);
if (embedding === "inherit")
return;
// This is based on the Frustum::enclose code from geom.cpp
var bbox = subscene.par3d.bbox,
scale = subscene.par3d.scale,
ranges = [(bbox[1]-bbox[0])*scale[0]/2,
(bbox[3]-bbox[2])*scale[1]/2,
(bbox[5]-bbox[4])*scale[2]/2],
radius = Math.sqrt(this.sumsq(ranges))*1.1; // A bit bigger to handle labels
if (radius <= 0) radius = 1;
var observer = subscene.par3d.observer,
distance = observer[2],
t = Math.tan(subscene.par3d.FOV*Math.PI/360),
near = distance - radius,
far = distance + radius,
hlen = t*near,
aspect = this.vp.width/this.vp.height,
z = subscene.par3d.zoom;
if (aspect > 1)
this.prMatrix.frustum(-hlen*aspect*z, hlen*aspect*z,
-hlen*z, hlen*z, near, far);
else
this.prMatrix.frustum(-hlen*z, hlen*z,
-hlen*z/aspect, hlen*z/aspect,
near, far);
};
this.setmvMatrix = function(id) {
var observer = this.getObj(id).par3d.observer;
this.mvMatrix.makeIdentity();
this.setmodelMatrix(id);
this.mvMatrix.translate(-observer[0], -observer[1], -observer[2]);
};
this.setmodelMatrix = function(id) {
var subscene = this.getObj(id),
embedding = subscene.embeddings.model;
if (embedding !== "inherit") {
var scale = subscene.par3d.scale,
bbox = subscene.par3d.bbox,
center = [(bbox[0]+bbox[1])/2,
(bbox[2]+bbox[3])/2,
(bbox[4]+bbox[5])/2];
this.mvMatrix.translate(-center[0], -center[1], -center[2]);
this.mvMatrix.scale(scale[0], scale[1], scale[2]);
this.mvMatrix.multRight( subscene.par3d.userMatrix );
}
if (embedding !== "replace")
this.setmodelMatrix(subscene.parent);
};
this.setnormMatrix = function(subsceneid) {
var self = this,
recurse = function(id) {
var sub = self.getObj(id),
embedding = sub.embeddings.model;
if (embedding !== "inherit") {
var scale = sub.par3d.scale;
self.normMatrix.scale(1/scale[0], 1/scale[1], 1/scale[2]);
self.normMatrix.multRight(sub.par3d.userMatrix);
}
if (embedding !== "replace")
recurse(sub.parent);
};
self.normMatrix.makeIdentity();
recurse(subsceneid);
};
this.setprmvMatrix = function() {
this.prmvMatrix = new CanvasMatrix4( this.mvMatrix );
this.prmvMatrix.multRight( this.prMatrix );
};
this.countClipplanes = function() {
return this.countObjs("clipplanes");
};
this.countLights = function() {
return this.countObjs("light");
};
this.countObjs = function(type) {
var self = this,
bound = 0;
Object.keys(this.scene.objects).forEach(
function(key) {
if (self.getObj(parseInt(key, 10)).type === type)
bound = bound + 1;
});
return bound;
};
this.initSubscene = function(id) {
var sub = this.getObj(id),
i, obj;
if (sub.type !== "subscene")
return;
sub.par3d.userMatrix = this.toCanvasMatrix4(sub.par3d.userMatrix);
sub.par3d.listeners = [].concat(sub.par3d.listeners);
sub.backgroundId = undefined;
sub.subscenes = [];
sub.clipplanes = [];
sub.transparent = [];
sub.opaque = [];
sub.lights = [];
for (i=0; i < sub.objects.length; i++) {
obj = this.getObj(sub.objects[i]);
if (typeof obj === "undefined") {
sub.objects.splice(i, 1);
i--;
} else if (obj.type === "background")
sub.backgroundId = obj.id;
else
sub[this.whichList(obj.id)].push(obj.id);
}
};
this.copyObj = function(id, reuse) {
var obj = this.getObj(id),
prev = document.getElementById(reuse).rglinstance,
prevobj = prev.getObj(id),
fields = ["flags", "type",
"colors", "vertices", "centers",
"normals", "offsets",
"texts", "cex", "family", "font", "adj",
"material",
"radii",
"texcoords",
"userMatrix", "ids",
"dim",
"par3d", "userMatrix",
"viewpoint", "finite"],
i;
for (i = 0; i < fields.length; i++) {
if (typeof prevobj[fields[i]] !== "undefined")
obj[fields[i]] = prevobj[fields[i]];
}
};
this.planeUpdateTriangles = function(id, bbox) {
var perms = [[0,0,1], [1,2,2], [2,1,0]],
x, xrow, elem, A, d, nhits, i, j, k, u, v, w, intersect, which, v0, v2, vx, reverse,
face1 = [], face2 = [], normals = [],
obj = this.getObj(id),
nPlanes = obj.normals.length;
obj.bbox = bbox;
obj.vertices = [];
obj.initialized = false;
for (elem = 0; elem < nPlanes; elem++) {
//    Vertex Av = normal.getRecycled(elem);
x = [];
A = obj.normals[elem];
d = obj.offsets[elem][0];
nhits = 0;
for (i=0; i<3; i++)
for (j=0; j<2; j++)
for (k=0; k<2; k++) {
u = perms[0][i];
v = perms[1][i];
w = perms[2][i];
if (A[w] != 0.0) {
intersect = -(d + A[u]*bbox[j+2*u] + A[v]*bbox[k+2*v])/A[w];
if (bbox[2*w] < intersect && intersect < bbox[1+2*w]) {
xrow = [];
xrow[u] = bbox[j+2*u];
xrow[v] = bbox[k+2*v];
xrow[w] = intersect;
x.push(xrow);
face1[nhits] = j + 2*u;
face2[nhits] = k + 2*v;
nhits++;
}
}
}
if (nhits > 3) {
/* Re-order the intersections so the triangles work */
for (i=0; i<nhits-2; i++) {
which = 0; /* initialize to suppress warning */
for (j=i+1; j<nhits; j++) {
if (face1[i] == face1[j] || face1[i] == face2[j]
|| face2[i] == face1[j] || face2[i] == face2[j] ) {
which = j;
break;
}
}
if (which > i+1) {
this.swap(x, i+1, which);
this.swap(face1, i+1, which);
this.swap(face2, i+1, which);
}
}
}
if (nhits >= 3) {
/* Put in order so that the normal points out the FRONT of the faces */
v0 = [x[0][0] - x[1][0] , x[0][1] - x[1][1], x[0][2] - x[1][2]];
v2 = [x[2][0] - x[1][0] , x[2][1] - x[1][1], x[2][2] - x[1][2]];
/* cross-product */
vx = this.xprod(v0, v2);
reverse = this.dotprod(vx, A) > 0;
for (i=0; i<nhits-2; i++) {
obj.vertices.push(x[0]);
normals.push(A);
for (j=1; j<3; j++) {
obj.vertices.push(x[i + (reverse ? 3-j : j)]);
normals.push(A);
}
}
}
}
obj.pnormals = normals;
};
this.initObj = function(id) {
var obj = this.getObj(id),
flags = obj.flags,
type = obj.type,
is_indexed = flags & this.f_is_indexed,
is_lit = flags & this.f_is_lit,
has_texture = flags & this.f_has_texture,
fixed_quads = flags & this.f_fixed_quads,
depth_sort = flags & this.f_depth_sort,
sprites_3d = flags & this.f_sprites_3d,
sprite_3d = flags & this.f_sprite_3d,
gl = this.gl,
texinfo, drawtype, nclipplanes, f, frowsize, nrows,
i,j,v, mat, uri, matobj;
if (typeof id !== "number") {
this.alertOnce("initObj id is "+typeof id);
}
obj.initialized = true;
if (type === "background" || type === "bboxdeco" || type === "subscene")
return;
if (type === "light") {
obj.ambient = new Float32Array(obj.colors[0].slice(0,3));
obj.diffuse = new Float32Array(obj.colors[1].slice(0,3));
obj.specular = new Float32Array(obj.colors[2].slice(0,3));
obj.lightDir = new Float32Array(obj.vertices[0]);
return;
}
if (type === "clipplanes") {
obj.vClipplane = this.flatten(this.cbind(obj.normals, obj.offsets));
return;
}
if (!sprites_3d) {
obj.prog = gl.createProgram();
gl.attachShader(obj.prog, this.getShader( gl.VERTEX_SHADER,
this.getVertexShader(id) ));
gl.attachShader(obj.prog, this.getShader( gl.FRAGMENT_SHADER,
this.getFragmentShader(id) ));
//  Force aPos to location 0, aCol to location 1
gl.bindAttribLocation(obj.prog, 0, "aPos");
gl.bindAttribLocation(obj.prog, 1, "aCol");
gl.linkProgram(obj.prog);
var linked = gl.getProgramParameter(obj.prog, gl.LINK_STATUS);
if (!linked) {
// An error occurred while linking
var lastError = gl.getProgramInfoLog(program);
console.warn("Error in program linking:" + lastError);
gl.deleteProgram(program);
}
}
if (type === "text") {
texinfo = this.drawTextToCanvas(obj.texts,
this.flatten(obj.cex),
this.flatten(obj.family),
this.flatten(obj.family));
}
if (fixed_quads && !sprites_3d) {
obj.ofsLoc = gl.getAttribLocation(obj.prog, "aOfs");
}
if (sprite_3d) {
obj.origLoc = gl.getUniformLocation(obj.prog, "uOrig");
obj.sizeLoc = gl.getUniformLocation(obj.prog, "uSize");
obj.usermatLoc = gl.getUniformLocation(obj.prog, "usermat");
}
if (has_texture || type == "text") {
obj.texture = gl.createTexture();
obj.texLoc = gl.getAttribLocation(obj.prog, "aTexcoord");
obj.sampler = gl.getUniformLocation(obj.prog, "uSampler");
}
if (has_texture) {
mat = obj.material;
if (typeof mat.uri !== "undefined")
uri = mat.uri;
else if (typeof mat.uriElementId === "undefined") {
matobj = this.getObj(mat.uriId);
if (typeof matobj !== "undefined") {
uri = matobj.material.uri;
} else {
uri = "";
}
} else
uri = document.getElementById(mat.uriElementId).rglinstance.getObj(mat.uriId).material.uri;
this.loadImageToTexture(uri, obj.texture);
}
if (type === "text") {
this.handleLoadedTexture(obj.texture, this.textureCanvas);
}
v = obj.vertices;
obj.vertexCount = v.length;
if (!obj.vertexCount) return;
var stride = 3, nc, cofs, nofs, radofs, oofs, tofs, vnew, v1;
nc = obj.colorCount = obj.colors.length;
if (nc > 1) {
cofs = stride;
stride = stride + 4;
v = this.cbind(v, obj.colors);
} else {
cofs = -1;
obj.onecolor = this.flatten(obj.colors);
}
if (typeof obj.normals !== "undefined") {
nofs = stride;
stride = stride + 3;
v = this.cbind(v, typeof obj.pnormals !== "undefined" ? obj.pnormals : obj.normals);
} else
nofs = -1;
if (typeof obj.radii !== "undefined") {
radofs = stride;
stride = stride + 1;
if (obj.radii.length === v.length) {
v = this.cbind(v, obj.radii);
} else if (obj.radii.length === 1) {
v = v.map(function(row, i, arr) { return row.concat(obj.radii[0]);});
}
} else
radofs = -1;
if (type == "sprites" && !sprites_3d) {
tofs = stride;
stride += 2;
oofs = stride;
stride += 2;
vnew = new Array(4*v.length);
var size = obj.radii, s = size[0]/2;
for (i=0; i < v.length; i++) {
if (size.length > 1)
s = size[i]/2;
vnew[4*i]  = v[i].concat([0,0,-s,-s]);
vnew[4*i+1]= v[i].concat([1,0, s,-s]);
vnew[4*i+2]= v[i].concat([1,1, s, s]);
vnew[4*i+3]= v[i].concat([0,1,-s, s]);
}
v = vnew;
obj.vertexCount = v.length;
} else if (type === "text") {
tofs = stride;
stride += 2;
oofs = stride;
stride += 2;
vnew = new Array(4*v.length);
for (i=0; i < v.length; i++) {
vnew[4*i]  = v[i].concat([0,-0.5]).concat(obj.adj[0]);
vnew[4*i+1]= v[i].concat([1,-0.5]).concat(obj.adj[0]);
vnew[4*i+2]= v[i].concat([1, 1.5]).concat(obj.adj[0]);
vnew[4*i+3]= v[i].concat([0, 1.5]).concat(obj.adj[0]);
for (j=0; j < 4; j++) {
v1 = vnew[4*i+j];
v1[tofs+2] = 2*(v1[tofs]-v1[tofs+2])*texinfo.widths[i];
v1[tofs+3] = 2*(v1[tofs+1]-v1[tofs+3])*texinfo.textHeights[i];
v1[tofs] *= texinfo.widths[i]/texinfo.canvasX;
v1[tofs+1] = 1.0-(texinfo.offsets[i] -
v1[tofs+1]*texinfo.textHeights[i])/texinfo.canvasY;
vnew[4*i+j] = v1;
}
}
v = vnew;
obj.vertexCount = v.length;
} else if (typeof obj.texcoords !== "undefined") {
tofs = stride;
stride += 2;
oofs = -1;
v = this.cbind(v, obj.texcoords);
} else {
tofs = -1;
oofs = -1;
}
if (stride !== v[0].length) {
this.alertOnce("problem in stride calculation");
}
obj.vOffsets = {vofs:0, cofs:cofs, nofs:nofs, radofs:radofs, oofs:oofs, tofs:tofs, stride:stride};
obj.values = new Float32Array(this.flatten(v));
if (sprites_3d) {
obj.userMatrix = new CanvasMatrix4(obj.userMatrix);
obj.objects = this.flatten([].concat(obj.ids));
is_lit = false;
}
if (is_lit && !fixed_quads) {
obj.normLoc = gl.getAttribLocation(obj.prog, "aNorm");
}
nclipplanes = this.countClipplanes();
if (nclipplanes && !sprites_3d) {
obj.clipLoc = [];
for (i=0; i < nclipplanes; i++)
obj.clipLoc[i] = gl.getUniformLocation(obj.prog,"vClipplane" + i);
}
if (is_lit) {
obj.emissionLoc = gl.getUniformLocation(obj.prog, "emission");
obj.emission = new Float32Array(this.stringToRgb(this.getMaterial(id, "emission")));
obj.shininessLoc = gl.getUniformLocation(obj.prog, "shininess");
obj.shininess = this.getMaterial(id, "shininess");
obj.nlights = this.countLights();
obj.ambientLoc = [];
obj.ambient = new Float32Array(this.stringToRgb(this.getMaterial(id, "ambient")));
obj.specularLoc = [];
obj.specular = new Float32Array(this.stringToRgb(this.getMaterial(id, "specular")));
obj.diffuseLoc = [];
obj.lightDirLoc = [];
obj.viewpointLoc = [];
obj.finiteLoc = [];
for (i=0; i < obj.nlights; i++) {
obj.ambientLoc[i] = gl.getUniformLocation(obj.prog, "ambient" + i);
obj.specularLoc[i] = gl.getUniformLocation(obj.prog, "specular" + i);
obj.diffuseLoc[i] = gl.getUniformLocation(obj.prog, "diffuse" + i);
obj.lightDirLoc[i] = gl.getUniformLocation(obj.prog, "lightDir" + i);
obj.viewpointLoc[i] = gl.getUniformLocation(obj.prog, "viewpoint" + i);
obj.finiteLoc[i] = gl.getUniformLocation(obj.prog, "finite" + i);
}
}
if (is_indexed) {
if ((type === "quads" || type === "text" ||
type === "sprites") && !sprites_3d) {
nrows = Math.floor(obj.vertexCount/4);
f = Array(6*nrows);
for (i=0; i < nrows; i++) {
f[6*i] = 4*i;
f[6*i+1] = 4*i + 1;
f[6*i+2] = 4*i + 2;
f[6*i+3] = 4*i;
f[6*i+4] = 4*i + 2;
f[6*i+5] = 4*i + 3;
}
frowsize = 6;
} else if (type === "triangles") {
nrows = Math.floor(obj.vertexCount/3);
f = Array(3*nrows);
for (i=0; i < f.length; i++) {
f[i] = i;
}
frowsize = 3;
} else if (type === "spheres") {
nrows = obj.vertexCount;
f = Array(nrows);
for (i=0; i < f.length; i++) {
f[i] = i;
}
frowsize = 1;
} else if (type === "surface") {
var dim = obj.dim[0],
nx = dim[0],
nz = dim[1];
f = [];
for (j=0; j<nx-1; j++) {
for (i=0; i<nz-1; i++) {
f.push(j + nx*i,
j + nx*(i+1),
j + 1 + nx*(i+1),
j + nx*i,
j + 1 + nx*(i+1),
j + 1 + nx*i);
}
}
frowsize = 6;
}
obj.f = new Uint16Array(f);
if (depth_sort) {
drawtype = "DYNAMIC_DRAW";
} else {
drawtype = "STATIC_DRAW";
}
}
if (type !== "spheres" && !sprites_3d) {
obj.buf = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, obj.buf);
gl.bufferData(gl.ARRAY_BUFFER, obj.values, gl.STATIC_DRAW); //
}
if (is_indexed && type !== "spheres" && !sprites_3d) {
obj.ibuf = gl.createBuffer();
gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, obj.ibuf);
gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, obj.f, gl[drawtype]);
}
if (!sprites_3d) {
obj.mvMatLoc = gl.getUniformLocation(obj.prog, "mvMatrix");
obj.prMatLoc = gl.getUniformLocation(obj.prog, "prMatrix");
}
if (type === "text") {
obj.textScaleLoc = gl.getUniformLocation(obj.prog, "textScale");
}
if (is_lit && !sprites_3d) {
obj.normMatLoc = gl.getUniformLocation(obj.prog, "normMatrix");
}
};
this.setDepthTest = function(id) {
var gl = this.gl,
tests = {never: gl.NEVER,
less:  gl.LESS,
equal: gl.EQUAL,
lequal:gl.LEQUAL,
greater: gl.GREATER,
notequal: gl.NOTEQUAL,
gequal: gl.GEQUAL,
always: gl.ALWAYS},
test = tests[this.getMaterial(id, "depth_test")];
gl.depthFunc(test);
};
this.mode4type = {points : "POINTS",
linestrip : "LINE_STRIP",
abclines : "LINES",
lines : "LINES",
sprites : "TRIANGLES",
planes : "TRIANGLES",
text : "TRIANGLES",
quads : "TRIANGLES",
surface : "TRIANGLES",
triangles : "TRIANGLES"};
this.drawObj = function(id, subsceneid) {
var obj = this.getObj(id),
subscene = this.getObj(subsceneid),
flags = obj.flags,
type = obj.type,
is_indexed = flags & this.f_is_indexed,
is_lit = flags & this.f_is_lit,
has_texture = flags & this.f_has_texture,
fixed_quads = flags & this.f_fixed_quads,
depth_sort = flags & this.f_depth_sort,
sprites_3d = flags & this.f_sprites_3d,
sprite_3d = flags & this.f_sprite_3d,
is_lines = flags & this.f_is_lines,
gl = this.gl,
sphereMV, baseofs, ofs, sscale, i, count, light,
faces;
if (typeof id !== "number") {
this.alertOnce("drawObj id is "+typeof id);
}
if (type === "planes") {
if (obj.bbox !== subscene.par3d.bbox || !obj.initialized) {
this.planeUpdateTriangles(id, subscene.par3d.bbox);
}
}
if (!obj.initialized)
this.initObj(id);
if (type === "light" || type === "bboxdeco")
return;
if (type === "clipplanes") {
count = obj.offsets.length;
var IMVClip = [];
for (i=0; i < count; i++) {
IMVClip[i] = this.multMV(this.invMatrix, obj.vClipplane.slice(4*i, 4*(i+1)));
}
obj.IMVClip = IMVClip;
return;
}
this.setDepthTest(id);
if (sprites_3d) {
var norigs = obj.vertices.length,
savenorm = new CanvasMatrix4(this.normMatrix);
this.origs = obj.vertices;
this.usermat = new Float32Array(obj.userMatrix.getAsArray());
this.radii = obj.radii;
this.normMatrix = subscene.spriteNormmat;
for (this.iOrig=0; this.iOrig < norigs; this.iOrig++) {
for (i=0; i < obj.objects.length; i++) {
this.drawObj(obj.objects[i], subsceneid);
}
}
this.normMatrix = savenorm;
return;
} else {
gl.useProgram(obj.prog);
}
if (sprite_3d) {
gl.uniform3fv(obj.origLoc, new Float32Array(this.origs[this.iOrig]));
if (this.radii.length > 1) {
gl.uniform1f(obj.sizeLoc, this.radii[this.iOrig][0]);
} else {
gl.uniform1f(obj.sizeLoc, this.radii[0][0]);
}
gl.uniformMatrix4fv(obj.usermatLoc, false, this.usermat);
}
if (type === "spheres") {
gl.bindBuffer(gl.ARRAY_BUFFER, this.sphere.buf);
} else {
gl.bindBuffer(gl.ARRAY_BUFFER, obj.buf);
}
if (is_indexed && type !== "spheres") {
gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, obj.ibuf);
} else if (type === "spheres") {
gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, this.sphere.ibuf);
}
gl.uniformMatrix4fv( obj.prMatLoc, false, new Float32Array(this.prMatrix.getAsArray()) );
gl.uniformMatrix4fv( obj.mvMatLoc, false, new Float32Array(this.mvMatrix.getAsArray()) );
var clipcheck = 0,
clipplaneids = subscene.clipplanes,
clip, j;
for (i=0; i < clipplaneids.length; i++) {
clip = this.getObj(clipplaneids[i]);
for (j=0; j < clip.offsets.length; j++) {
gl.uniform4fv(obj.clipLoc[clipcheck + j], clip.IMVClip[j]);
}
clipcheck += clip.offsets.length;
}
if (typeof obj.clipLoc !== "undefined")
for (i=clipcheck; i < obj.clipLoc.length; i++)
gl.uniform4f(obj.clipLoc[i], 0,0,0,0);
if (is_lit) {
gl.uniformMatrix4fv( obj.normMatLoc, false, new Float32Array(this.normMatrix.getAsArray()) );
gl.uniform3fv( obj.emissionLoc, obj.emission);
gl.uniform1f( obj.shininessLoc, obj.shininess);
for (i=0; i < subscene.lights.length; i++) {
light = this.getObj(subscene.lights[i]);
gl.uniform3fv( obj.ambientLoc[i], this.componentProduct(light.ambient, obj.ambient));
gl.uniform3fv( obj.specularLoc[i], this.componentProduct(light.specular, obj.specular));
gl.uniform3fv( obj.diffuseLoc[i], light.diffuse);
gl.uniform3fv( obj.lightDirLoc[i], light.lightDir);
gl.uniform1i( obj.viewpointLoc[i], light.viewpoint);
gl.uniform1i( obj.finiteLoc[i], light.finite);
}
for (i=subscene.lights.length; i < obj.nlights; i++) {
gl.uniform3f( obj.ambientLoc[i], 0,0,0);
gl.uniform3f( obj.specularLoc[i], 0,0,0);
gl.uniform3f( obj.diffuseLoc[i], 0,0,0);
}
}
if (type === "text") {
gl.uniform2f( obj.textScaleLoc, 0.75/this.vp.width, 0.75/this.vp.height);
}
gl.enableVertexAttribArray( this.posLoc );
var nc = obj.colorCount;
count = obj.vertexCount;
if (depth_sort) {
var nfaces = obj.centers.length,
frowsize, z, w;
if (sprites_3d) frowsize = 1;
else if (type === "triangles") frowsize = 3;
else frowsize = 6;
var depths = new Float32Array(nfaces);
faces = new Array(nfaces);
for(i=0; i<nfaces; i++) {
z = this.prmvMatrix.m13*obj.centers[3*i] +
this.prmvMatrix.m23*obj.centers[3*i+1] +
this.prmvMatrix.m33*obj.centers[3*i+2] +
this.prmvMatrix.m43;
w = this.prmvMatrix.m14*obj.centers[3*i] +
this.prmvMatrix.m24*obj.centers[3*i+1] +
this.prmvMatrix.m34*obj.centers[3*i+2] +
this.prmvMatrix.m44;
depths[i] = z/w;
faces[i] = i;
}
var depthsort = function(i,j) { return depths[j] - depths[i]; };
faces.sort(depthsort);
if (type !== "spheres") {
var f = new Uint16Array(obj.f.length);
for (i=0; i<nfaces; i++) {
for (j=0; j<frowsize; j++) {
f[frowsize*i + j] = obj.f[frowsize*faces[i] + j];
}
}
gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, f, gl.DYNAMIC_DRAW);
}
}
if (type === "spheres") {
subscene = this.getObj(subsceneid);
var scale = subscene.par3d.scale,
scount = count;
gl.vertexAttribPointer(this.posLoc,  3, gl.FLOAT, false, this.sphere.sphereStride,  0);
gl.enableVertexAttribArray(obj.normLoc );
gl.vertexAttribPointer(obj.normLoc,  3, gl.FLOAT, false, this.sphere.sphereStride,  0);
gl.disableVertexAttribArray( this.colLoc );
var sphereNorm = new CanvasMatrix4();
sphereNorm.scale(scale[0], scale[1], scale[2]);
sphereNorm.multRight(this.normMatrix);
gl.uniformMatrix4fv( obj.normMatLoc, false, new Float32Array(sphereNorm.getAsArray()) );
if (nc == 1) {
gl.vertexAttrib4fv( this.colLoc, new Float32Array(obj.onecolor));
}
for (i = 0; i < scount; i++) {
sphereMV = new CanvasMatrix4();
if (depth_sort) {
baseofs = faces[i]*obj.vOffsets.stride;
} else {
baseofs = i*obj.vOffsets.stride;
}
ofs = baseofs + obj.vOffsets.radofs;
sscale = obj.values[ofs];
sphereMV.scale(sscale/scale[0], sscale/scale[1], sscale/scale[2]);
sphereMV.translate(obj.values[baseofs],
obj.values[baseofs+1],
obj.values[baseofs+2]);
sphereMV.multRight(this.mvMatrix);
gl.uniformMatrix4fv( obj.mvMatLoc, false, new Float32Array(sphereMV.getAsArray()) );
if (nc > 1) {
ofs = baseofs + obj.vOffsets.cofs;
gl.vertexAttrib4f( this.colLoc, obj.values[ofs],
obj.values[ofs+1],
obj.values[ofs+2],
obj.values[ofs+3] );
}
gl.drawElements(gl.TRIANGLES, this.sphere.sphereCount, gl.UNSIGNED_SHORT, 0);
}
return;
} else {
if (obj.colorCount === 1) {
gl.disableVertexAttribArray( this.colLoc );
gl.vertexAttrib4fv( this.colLoc, new Float32Array(obj.onecolor));
} else {
gl.enableVertexAttribArray( this.colLoc );
gl.vertexAttribPointer(this.colLoc, 4, gl.FLOAT, false, 4*obj.vOffsets.stride, 4*obj.vOffsets.cofs);
}
}
if (is_lit && obj.vOffsets.nofs > 0) {
gl.enableVertexAttribArray( obj.normLoc );
gl.vertexAttribPointer(obj.normLoc, 3, gl.FLOAT, false, 4*obj.vOffsets.stride, 4*obj.vOffsets.nofs);
}
if (has_texture || type === "text") {
gl.enableVertexAttribArray( obj.texLoc );
gl.vertexAttribPointer(obj.texLoc, 2, gl.FLOAT, false, 4*obj.vOffsets.stride, 4*obj.vOffsets.tofs);
gl.activeTexture(gl.TEXTURE0);
gl.bindTexture(gl.TEXTURE_2D, obj.texture);
gl.uniform1i( obj.sampler, 0);
}
if (fixed_quads) {
gl.enableVertexAttribArray( obj.ofsLoc );
gl.vertexAttribPointer(obj.ofsLoc, 2, gl.FLOAT, false, 4*obj.vOffsets.stride, 4*obj.vOffsets.oofs);
}
var mode = this.mode4type[type];
if (type === "sprites" || type === "text" || type === "quads") {
count = count * 6/4;
} else if (type === "surface") {
count = obj.f.length;
}
if (is_lines) {
gl.lineWidth( this.getMaterial(id, "lwd") );
}
gl.vertexAttribPointer(this.posLoc,  3, gl.FLOAT, false, 4*obj.vOffsets.stride,  4*obj.vOffsets.vofs);
if (is_indexed) {
gl.drawElements(gl[mode], count, gl.UNSIGNED_SHORT, 0);
} else {
gl.drawArrays(gl[mode], 0, count);
}
};
this.drawSubscene = function(subsceneid) {
var gl = this.gl,
obj = this.getObj(subsceneid),
objects = this.scene.objects,
subids = obj.objects,
subscene_has_faces = false,
subscene_needs_sorting = false,
flags, i;
if (obj.par3d.skipRedraw)
return;
for (i=0; i < subids.length; i++) {
flags = objects[subids[i]].flags;
if (typeof flags !== "undefined") {
subscene_has_faces |= (flags & this.f_is_lit)
& !(flags & this.f_fixed_quads);
subscene_needs_sorting |= (flags & this.f_depth_sort);
}
}
var bgid = obj.backgroundId,
bg;
this.setViewport(subsceneid);
if (typeof bgid !== "undefined" && objects[bgid].colors.length) {
bg = objects[bgid].colors[0];
gl.clearColor(bg[0], bg[1], bg[2], bg[3]);
gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
}
if (subids.length) {
this.setprMatrix(subsceneid);
this.setmvMatrix(subsceneid);
if (subscene_has_faces) {
this.setnormMatrix(subsceneid);
if ((obj.flags & this.f_sprites_3d) &&
typeof obj.spriteNormmat === "undefined") {
obj.spriteNormmat = new CanvasMatrix4(this.normMatrix);
}
}
if (subscene_needs_sorting)
this.setprmvMatrix();
var clipids = obj.clipplanes;
if (clipids.length > 0) {
this.invMatrix = new CanvasMatrix4(this.mvMatrix);
this.invMatrix.invert();
for (i = 0; i < clipids.length; i++)
this.drawObj(clipids[i], subsceneid);
}
subids = obj.opaque;
if (subids.length > 0) {
gl.depthMask(true);
gl.disable(gl.BLEND);
for (i = 0; subids && i < subids.length; i++) {
this.drawObj(subids[i], subsceneid);
}
}
subids = obj.transparent;
if (subids.length > 0) {
gl.depthMask(false);
gl.blendFuncSeparate(gl.SRC_ALPHA, gl.ONE_MINUS_SRC_ALPHA,
gl.ONE, gl.ONE);
gl.enable(gl.BLEND);
for (i = 0; i < subids.length; i++) {
this.drawObj(subids[i], subsceneid);
}
}
subids = obj.subscenes;
for (i = 0; i < subids.length; i++) {
this.drawSubscene(subids[i]);
}
}
};
this.relMouseCoords = function(event) {
var totalOffsetX = 0,
totalOffsetY = 0,
currentElement = this.canvas;
do {
totalOffsetX += currentElement.offsetLeft;
totalOffsetY += currentElement.offsetTop;
currentElement = currentElement.offsetParent;
}
while(currentElement);
var canvasX = event.pageX - totalOffsetX,
canvasY = event.pageY - totalOffsetY;
return {x:canvasX, y:canvasY};
};
this.setMouseHandlers = function() {
var self = this, activeSubscene, handler,
handlers = {}, drag = 0;
handlers.rotBase = 0;
this.screenToVector = function(x, y) {
var viewport = this.getObj(activeSubscene).par3d.viewport,
width = viewport.width*this.canvas.width,
height = viewport.height*this.canvas.height,
radius = Math.max(width, height)/2.0,
cx = width/2.0,
cy = height/2.0,
px = (x-cx)/radius,
py = (y-cy)/radius,
plen = Math.sqrt(px*px+py*py);
if (plen > 1.e-6) {
px = px/plen;
py = py/plen;
}
var angle = (Math.SQRT2 - plen)/Math.SQRT2*Math.PI/2,
z = Math.sin(angle),
zlen = Math.sqrt(1.0 - z*z);
px = px * zlen;
py = py * zlen;
return [px, py, z];
};
handlers.trackballdown = function(x,y) {
var activeSub = this.getObj(activeSubscene),
activeModel = this.getObj(this.useid(activeSub.id, "model")),
i, l = activeModel.par3d.listeners;
handlers.rotBase = this.screenToVector(x, y);
this.saveMat = [];
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.saveMat = new CanvasMatrix4(activeSub.par3d.userMatrix);
}
};
handlers.trackballmove = function(x,y) {
var rotCurrent = this.screenToVector(x,y),
rotBase = handlers.rotBase,
dot = rotBase[0]*rotCurrent[0] +
rotBase[1]*rotCurrent[1] +
rotBase[2]*rotCurrent[2],
angle = Math.acos( dot/this.vlen(rotBase)/this.vlen(rotCurrent) )*180.0/Math.PI,
axis = this.xprod(rotBase, rotCurrent),
objects = this.scene.objects,
activeSub = this.getObj(activeSubscene),
activeModel = this.getObj(this.useid(activeSub.id, "model")),
l = activeModel.par3d.listeners,
i;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.par3d.userMatrix.load(objects[l[i]].saveMat);
activeSub.par3d.userMatrix.rotate(angle, axis[0], axis[1], axis[2]);
}
this.drawScene();
};
handlers.trackballend = 0;
handlers.axisdown = function(x,y) {
handlers.rotBase = this.screenToVector(x, this.canvas.height/2);
var activeSub = this.getObj(activeSubscene),
activeModel = this.getObj(this.useid(activeSub.id, "model")),
i, l = activeModel.par3d.listeners;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.saveMat = new CanvasMatrix4(activeSub.par3d.userMatrix);
}
};
handlers.axismove = function(x,y) {
var rotCurrent = this.screenToVector(x, this.canvas.height/2),
rotBase = handlers.rotBase,
angle = (rotCurrent[0] - rotBase[0])*180/Math.PI,
rotMat = new CanvasMatrix4();
rotMat.rotate(angle, handlers.axis[0], handlers.axis[1], handlers.axis[2]);
var activeSub = this.getObj(activeSubscene),
activeModel = this.getObj(this.useid(activeSub.id, "model")),
i, l = activeModel.par3d.listeners;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.par3d.userMatrix.load(activeSub.saveMat);
activeSub.par3d.userMatrix.multLeft(rotMat);
}
this.drawScene();
};
handlers.axisend = 0;
handlers.y0zoom = 0;
handlers.zoom0 = 0;
handlers.zoomdown = function(x, y) {
var activeSub = this.getObj(activeSubscene),
activeProjection = this.getObj(this.useid(activeSub.id, "projection")),
i, l = activeProjection.par3d.listeners;
handlers.y0zoom = y;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.zoom0 = Math.log(activeSub.par3d.zoom);
}
};
handlers.zoommove = function(x, y) {
var activeSub = this.getObj(activeSubscene),
activeProjection = this.getObj(this.useid(activeSub.id, "projection")),
i, l = activeProjection.par3d.listeners;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.par3d.zoom = Math.exp(activeSub.zoom0 + (y-handlers.y0zoom)/this.canvas.height);
}
this.drawScene();
};
handlers.zoomend = 0;
handlers.y0fov = 0;
handlers.fovdown = function(x, y) {
handlers.y0fov = y;
var activeSub = this.getObj(activeSubscene),
activeProjection = this.getObj(this.useid(activeSub.id, "projection")),
i, l = activeProjection.par3d.listeners;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.fov0 = activeSub.par3d.FOV;
}
};
handlers.fovmove = function(x, y) {
var activeSub = this.getObj(activeSubscene),
activeProjection = this.getObj(this.useid(activeSub.id, "projection")),
i, l = activeProjection.par3d.listeners;
for (i = 0; i < l.length; i++) {
activeSub = this.getObj(l[i]);
activeSub.par3d.FOV = Math.max(1, Math.min(179, activeSub.fov0 +
180*(y-handlers.y0fov)/this.canvas.height));
}
this.drawScene();
};
handlers.fovend = 0;
this.canvas.onmousedown = function ( ev ){
if (!ev.which) // Use w3c defns in preference to MS
switch (ev.button) {
case 0: ev.which = 1; break;
case 1:
case 4: ev.which = 2; break;
case 2: ev.which = 3;
}
drag = ["left", "middle", "right"][ev.which-1];
var coords = self.relMouseCoords(ev);
coords.y = self.canvas.height-coords.y;
activeSubscene = self.whichSubscene(coords);
var sub = self.getObj(activeSubscene), f;
handler = sub.par3d.mouseMode[drag];
switch (handler) {
case "xAxis":
handler = "axis";
handlers.axis = [1.0, 0.0, 0.0];
break;
case "yAxis":
handler = "axis";
handlers.axis = [0.0, 1.0, 0.0];
break;
case "zAxis":
handler = "axis";
handlers.axis = [0.0, 0.0, 1.0];
break;
}
f = handlers[handler + "down"];
if (f) {
coords = self.translateCoords(activeSubscene, coords);
f.call(self, coords.x, coords.y);
ev.preventDefault();
}
};
this.canvas.onmouseup = function ( ev ){
if ( drag === 0 ) return;
var f = handlers[handler + "up"];
if (f)
f();
drag = 0;
};
this.canvas.onmouseout = this.canvas.onmouseup;
this.canvas.onmousemove = function ( ev ) {
if ( drag === 0 ) return;
var f = handlers[handler + "move"];
if (f) {
var coords = self.relMouseCoords(ev);
coords.y = self.canvas.height - coords.y;
coords = self.translateCoords(activeSubscene, coords);
f.call(self, coords.x, coords.y);
}
};
handlers.wheelHandler = function(ev) {
var del = 1.02, i;
if (ev.shiftKey) del = 1.002;
var ds = ((ev.detail || ev.wheelDelta) > 0) ? del : (1 / del);
if (typeof activeSubscene === "undefined")
activeSubscene = self.scene.rootSubscene;
var activeSub = self.getObj(activeSubscene),
activeProjection = self.getObj(self.useid(activeSub.id, "projection")),
l = activeProjection.par3d.listeners;
for (i = 0; i < l.length; i++) {
activeSub = self.getObj(l[i]);
activeSub.par3d.zoom *= ds;
}
self.drawScene();
ev.preventDefault();
};
this.canvas.addEventListener("DOMMouseScroll", handlers.wheelHandler, false);
this.canvas.addEventListener("mousewheel", handlers.wheelHandler, false);
};
this.useid = function(subsceneid, type) {
var sub = this.getObj(subsceneid);
if (sub.embeddings[type] === "inherit")
return(this.useid(sub.parent, type));
else
return subsceneid;
};
this.inViewport = function(coords, subsceneid) {
var viewport = this.getObj(subsceneid).par3d.viewport,
x0 = coords.x - viewport.x*this.canvas.width,
y0 = coords.y - viewport.y*this.canvas.height;
return 0 <= x0 && x0 <= viewport.width*this.canvas.width &&
0 <= y0 && y0 <= viewport.height*this.canvas.height;
};
this.whichSubscene = function(coords) {
var self = this,
recurse = function(subsceneid) {
var subscenes = self.getChildSubscenes(subsceneid), i, id;
for (i=0; i < subscenes.length; i++) {
id = recurse(subscenes[i]);
if (typeof(id) !== "undefined")
return(id);
}
if (self.inViewport(coords, subsceneid))
return(subsceneid);
else
return undefined;
},
rootid = this.scene.rootSubscene,
result = recurse(rootid);
if (typeof(result) === "undefined")
result = rootid;
return result;
};
this.translateCoords = function(subsceneid, coords) {
var viewport = this.getObj(subsceneid).par3d.viewport;
return {x: coords.x - viewport.x*this.canvas.width,
y: coords.y - viewport.y*this.canvas.height};
};
this.initSphere = function(verts) {
var gl = this.gl, reuse = verts.reuse, result;
if (typeof reuse !== "undefined") {
var prev = document.getElementById(reuse).rglinstance.sphere;
result = {vb: prev.vb, it: prev.it};
} else {
result = {vb: new Float32Array(this.flatten(this.transpose(verts.vb))),
it: new Uint16Array(this.flatten(this.transpose(verts.it)))};
}
result.sphereStride = 12;
result.sphereCount = result.it.length;
result.buf = gl.createBuffer();
gl.bindBuffer(gl.ARRAY_BUFFER, result.buf);
gl.bufferData(gl.ARRAY_BUFFER, result.vb, gl.STATIC_DRAW);
result.ibuf = gl.createBuffer();
gl.bindBuffer(gl.ELEMENT_ARRAY_BUFFER, result.ibuf);
gl.bufferData(gl.ELEMENT_ARRAY_BUFFER, result.it, gl.STATIC_DRAW);
return result;
};
this.initialize = function(el, x) {
this.textureCanvas = document.createElement("canvas");
this.textureCanvas.style.display = "block";
this.scene = x;
this.normMatrix = new CanvasMatrix4();
this.saveMat = {};
this.distance = null;
this.posLoc = 0;
this.colLoc = 1;
if (el) {
el.rglinstance = this;
this.initCanvas(el);
}
};
this.initCanvas = function(el) {
this.canvas = document.createElement("canvas");
this.resize(el);
while (el.firstChild) {
el.removeChild(el.firstChild);
}
el.appendChild(this.canvas);
this.initGL0();
if (!this.gl)
return;
var objs = this.scene.objects,
self = this;
this.sphere = this.initSphere(this.scene.sphereVerts);
Object.keys(objs).forEach(function(key){
var id = parseInt(key, 10),
obj = self.getObj(id);
if (typeof obj.reuse !== "undefined")
self.copyObj(id, obj.reuse);
});
Object.keys(objs).forEach(function(key){
self.initSubscene(parseInt(key, 10));
});
Object.keys(objs).forEach(function(key){
self.initObj(parseInt(key, 10));
});
this.setMouseHandlers();
};
/* this is only used by .writeWebGL; rglwidget has
no debug element and does the drawing in rglwidget.js */
this.start = function() {
if (typeof this.prefix !== "undefined") {
this.debugelement = document.getElementById(this.prefix + "debug");
this.debug("");
}
this.drag = 0;
this.drawScene();
};
this.debug = function(msg, img) {
if (typeof this.debugelement !== "undefined") {
this.debugelement.innerHTML = msg;
if (typeof img !== "undefined") {
this.debugelement.insertBefore(img, this.debugelement.firstChild);
}
} else
alert(msg);
};
this.getSnapshot = function() {
var img;
if (typeof this.scene.snapshot !== "undefined") {
img = document.createElement("img");
img.src = this.scene.snapshot;
img.alt = "Snapshot";
}
return img;
};
this.initGL0 = function() {
if (!window.WebGLRenderingContext){
this.debug("Your browser does not support WebGL. See <a href=\"http://get.webgl.org\">http://get.webgl.org</a>", this.getSnapshot());
return;
}
try {
this.initGL();
}
catch(e) {}
if ( !this.gl ) {
this.debug("Your browser appears to support WebGL, but did not create a WebGL context.  See <a href=\"http://get.webgl.org\">http://get.webgl.org</a>",
this.getSnapshot());
return;
}
};
this.initGL = function() {
this.gl = this.canvas.getContext("webgl") ||
this.canvas.getContext("experimental-webgl");
};
this.resize = function(el) {
this.canvas.width = el.width;
this.canvas.height = el.height;
};
this.drawScene = function() {
var gl = this.gl;
if (!gl)
this.alertOnce("No WebGL context.");
gl.enable(gl.DEPTH_TEST);
gl.depthFunc(gl.LEQUAL);
gl.clearDepth(1.0);
gl.clearColor(1,1,1,1);
gl.clear(gl.COLOR_BUFFER_BIT | gl.DEPTH_BUFFER_BIT);
this.drawSubscene(this.scene.rootSubscene);
this.drawing = false;
};
this.subsetSetter = function(el, control) {
if (typeof control.subscenes === "undefined" ||
control.subscenes === null)
control.subscenes = this.scene.rootSubscene;
var value = Math.round(control.value),
subscenes = [].concat(control.subscenes),
i, j, entries, subsceneid,
ismissing = function(x) {
return control.fullset.indexOf(x) < 0;
},
tointeger = function(x) {
return parseInt(x, 10);
};
for (i=0; i < subscenes.length; i++) {
subsceneid = subscenes[i];
if (typeof this.getObj(subsceneid) === "undefined")
this.alertOnce("typeof object is undefined");
entries = this.getObj(subsceneid).objects;
entries = entries.filter(ismissing);
if (control.accumulate) {
for (j=0; j<=value; j++)
entries = entries.concat(control.subsets[j]);
} else {
entries = entries.concat(control.subsets[value]);
}
entries = entries.map(tointeger);
this.setSubsceneEntries(this.unique(entries), subsceneid);
}
};
this.propertySetter = function(el, control)  {
var value = control.value,
values = [].concat(control.values),
svals = [].concat(control.param),
direct = values[0] === null,
entries = [].concat(control.entries),
ncol = entries.length,
nrow = values.length/ncol,
properties = this.repeatToLen(control.properties, ncol),
objids = this.repeatToLen(control.objids, ncol),
property = properties[0], objid = objids[0],
obj = this.getObj(objid),
propvals, i, v1, v2, p, entry, gl, needsBinding,
newprop, newid,
getPropvals = function() {
if (property === "userMatrix")
return obj.userMatrix.getAsArray();
else
return obj[property];
};
if (direct && typeof value === "undefined")
return;
if (control.interp) {
values = values.slice(0, ncol).concat(values).
concat(values.slice(ncol*(nrow-1), ncol*nrow));
svals = [-Infinity].concat(svals).concat(Infinity);
for (i = 1; i < svals.length; i++) {
if (value <= svals[i]) {
if (svals[i] === Infinity)
p = 1;
else
p = (svals[i] - value)/(svals[i] - svals[i-1]);
break;
}
}
} else if (!direct) {
value = Math.round(value);
}
propvals = getPropvals();
for (j=0; j<entries.length; j++) {
entry = entries[j];
newprop = properties[j];
newid = objids[j];
if (newprop != property || newid != objid) {
property = newprop;
objid = newid;
obj = this.getObj(objid);
propvals = getPropvals();
}
if (control.interp) {
v1 = values[ncol*(i-1) + j];
v2 = values[ncol*i + j];
this.setElement(propvals, entry, p*v1 + (1-p)*v2);
} else if (!direct) {
this.setElement(propvals, entry, values[ncol*value + j]);
} else {
this.setElement(propvals, entry, value[j]);
}
}
needsBinding = [];
for (j=0; j < entries.length; j++) {
if (properties[j] === "values" &&
needsBinding.indexOf(objids[j]) === -1) {
needsBinding.push(objids[j]);
}
}
for (j=0; j < needsBinding.length; j++) {
gl = this.gl;
obj = this.getObj(needsBinding[j]);
gl.bindBuffer(gl.ARRAY_BUFFER, obj.buf);
gl.bufferData(gl.ARRAY_BUFFER, obj.values, gl.STATIC_DRAW);
}
};
this.vertexSetter = function(el, control)  {
var svals = [].concat(control.param),
j, k, p, propvals, stride, ofs, obj,
attrib,
ofss    = {x:"vofs", y:"vofs", z:"vofs",
red:"cofs", green:"cofs", blue:"cofs",
alpha:"cofs", radii:"radofs",
nx:"nofs", ny:"nofs", nz:"nofs",
ox:"oofs", oy:"oofs", oz:"oofs",
ts:"tofs", tt:"tofs"},
pos     = {x:0, y:1, z:2,
red:0, green:1, blue:2,
alpha:3,radii:0,
nx:0, ny:1, nz:2,
ox:0, oy:1, oz:2,
ts:0, tt:1},
values = control.values,
direct = values === null,
ncol,
interp = control.interp,
vertices = [].concat(control.vertices),
attributes = [].concat(control.attributes),
value = control.value;
ncol = Math.max(vertices.length, attributes.length);
if (!ncol)
return;
vertices = this.repeatToLen(vertices, ncol);
attributes = this.repeatToLen(attributes, ncol);
if (direct)
interp = false;
/* JSON doesn't pass Infinity */
svals[0] = -Infinity;
svals[svals.length - 1] = Infinity;
for (j = 1; j < svals.length; j++) {
if (value <= svals[j]) {
if (interp) {
if (svals[j] === Infinity)
p = 1;
else
p = (svals[j] - value)/(svals[j] - svals[j-1]);
} else {
if (svals[j] - value > value - svals[j-1])
j = j - 1;
}
break;
}
}
obj = this.getObj(control.objid);
propvals = obj.values;
for (k=0; k<ncol; k++) {
attrib = attributes[k];
vertex = vertices[k];
ofs = obj.vOffsets[ofss[attrib]];
if (ofs < 0)
this.alertOnce("Attribute '"+attrib+"' not found in object "+control.objid);
else {
stride = obj.vOffsets.stride;
ofs = vertex*stride + ofs + pos[attrib];
if (direct) {
propvals[ofs] = value;
} else if (interp) {
propvals[ofs] = p*values[j-1][k] + (1-p)*values[j][k];
} else {
propvals[ofs] = values[j][k];
}
}
}
if (typeof obj.buf !== "undefined") {
var gl = this.gl;
gl.bindBuffer(gl.ARRAY_BUFFER, obj.buf);
gl.bufferData(gl.ARRAY_BUFFER, propvals, gl.STATIC_DRAW);
}
};
this.ageSetter = function(el, control) {
var objids = [].concat(control.objids),
nobjs = objids.length,
time = control.value,
births = [].concat(control.births),
ages = [].concat(control.ages),
steps = births.length,
j = Array(steps),
p = Array(steps),
i, k, age, j0, propvals, stride, ofs, objid, obj,
attrib, dim,
attribs = ["colors", "alpha", "radii", "vertices",
"normals", "origins", "texcoords",
"x", "y", "z",
"red", "green", "blue"],
ofss    = ["cofs", "cofs", "radofs", "vofs",
"nofs", "oofs", "tofs",
"vofs", "vofs", "vofs",
"cofs", "cofs", "cofs"],
dims    = [3,1,1,3,
3,2,2,
1,1,1,
1,1,1],
pos     = [0,3,0,0,
0,0,0,
0,1,2,
0,1,2];
/* Infinity doesn't make it through JSON */
ages[0] = -Infinity;
ages[ages.length-1] = Infinity;
for (i = 0; i < steps; i++) {
if (births[i] !== null) {  // NA in R becomes null
age = time - births[i];
for (j0 = 1; age > ages[j0]; j0++);
if (ages[j0] == Infinity)
p[i] = 1;
else if (ages[j0] > ages[j0-1])
p[i] = (ages[j0] - age)/(ages[j0] - ages[j0-1]);
else
p[i] = 0;
j[i] = j0;
}
}
for (l = 0; l < nobjs; l++) {
objid = objids[l];
obj = this.getObj(objid);
propvals = obj.values;
stride = obj.vOffsets.stride;
for (k = 0; k < attribs.length; k++) {
attrib = control[attribs[k]];
if (typeof attrib !== "undefined") {
ofs = obj.vOffsets[ofss[k]];
if (ofs >= 0) {
dim = dims[k];
ofs = ofs + pos[k];
for (i = 0; i < steps; i++) {
if (births[i] !== null) {
for (d=0; d < dim; d++) {
propvals[i*stride + ofs + d] = p[i]*attrib[dim*(j[i]-1) + d] + (1-p[i])*attrib[dim*j[i] + d];
}
}
}
} else
this.alertOnce("\'"+attribs[k]+"\' property not found in object "+objid);
}
}
obj.values = propvals;
if (typeof obj.buf !== "undefined") {
gl = this.gl;
gl.bindBuffer(gl.ARRAY_BUFFER, obj.buf);
gl.bufferData(gl.ARRAY_BUFFER, obj.values, gl.STATIC_DRAW);
}
}
};
this.oldBridge = function(el, control) {
var attrname, global = window[control.prefix + "rgl"];
if (typeof global !== "undefined")
for (attrname in global)
this[attrname] = global[attrname];
window[control.prefix + "rgl"] = this;
};
this.Player = function(el, control) {
var
self = this,
components = [].concat(control.components),
Tick = function() { /* "this" will be a timer */
var i,
nominal = this.value,
slider = this.Slider,
labels = this.outputLabels,
output = this.Output,
step;
if (typeof slider !== "undefined" && nominal != slider.value)
slider.value = nominal;
if (typeof output !== "undefined") {
step = Math.round((nominal - output.sliderMin)/output.sliderStep);
if (labels !== null) {
output.innerHTML = labels[step];
} else {
step = step*output.sliderStep + output.sliderMin;
output.innerHTML = step.toPrecision(output.outputPrecision);
}
}
for (i=0; i < this.actions.length; i++) {
this.actions[i].value = nominal;
}
self.applyControls(el, this.actions, false);
self.drawScene();
},
OnSliderInput = function() { /* "this" will be the slider */
this.rgltimer.value = Number(this.value);
this.rgltimer.Tick();
},
addSlider = function(min, max, step, value) {
var slider = document.createElement("input");
slider.type = "range";
slider.min = min;
slider.max = max;
slider.step = step;
slider.value = value;
slider.oninput = OnSliderInput;
slider.sliderActions = control.actions;
slider.sliderScene = this;
slider.className = "rgl-slider";
slider.id = el.id + "-slider";
el.rgltimer.Slider = slider;
slider.rgltimer = el.rgltimer;
el.appendChild(slider);
},
addLabel = function(labels, min, step, precision) {
var output = document.createElement("output");
output.sliderMin = min;
output.sliderStep = step;
output.outputPrecision = precision;
output.className = "rgl-label";
output.id = el.id + "-label";
el.rgltimer.Output = output;
el.rgltimer.outputLabels = labels;
el.appendChild(output);
},
addButton = function(label) {
var button = document.createElement("input"),
onclicks = {Reverse: function() { this.rgltimer.reverse();},
Play: function() { this.rgltimer.play();
this.value = this.rgltimer.enabled ? "Pause" : "Play"; },
Slower: function() { this.rgltimer.slower(); },
Faster: function() { this.rgltimer.faster(); },
Reset: function() { this.rgltimer.reset(); }};
button.rgltimer = el.rgltimer;
button.type = "button";
button.value = label;
if (label === "Play")
button.rgltimer.PlayButton = button;
button.onclick = onclicks[label];
button.className = "rgl-button";
button.id = el.id + "-" + label;
el.appendChild(button);
};
if (typeof control.reinit !== "null") {
control.actions.reinit = control.reinit;
}
el.rgltimer = new rgltimerClass(Tick, control.start, control.interval, control.stop, control.value, control.rate, control.loop, control.actions);
for (var i=0; i < components.length; i++) {
switch(components[i]) {
case "Slider": addSlider(control.start, control.stop,
control.step, control.value);
break;
case "Label": addLabel(control.labels, control.start,
control.step, control.precision);
break;
default:
addButton(components[i]);
}
}
el.rgltimer.Tick();
};
this.applyControls = function(el, x, draw) {
var self = this, reinit = x.reinit, i, obj, control, type;
for (i = 0; i < x.length; i++) {
control = x[i];
type = control.type;
self[type](el, control);
};
if (typeof reinit !== "undefined" && reinit !== null) {
reinit = [].concat(reinit);
for (i = 0; i < reinit.length; i++)
self.getObj(reinit[i]).initialized = false;
}
if (typeof draw === "undefined" || draw)
self.drawScene();
};
this.sceneChangeHandler = function(message) {
var self = document.getElementById(message.elementId).rglinstance,
objs = message.objects, mat = message.material,
root = message.rootSubscene,
initSubs = message.initSubscenes,
redraw = message.redrawScene,
skipRedraw = message.skipRedraw,
deletes, subs, allsubs = [], obj, i,j;
if (typeof message.delete !== "undefined") {
deletes = [].concat(message.delete);
if (typeof message.delfromSubscenes !== "undefined")
subs = [].concat(message.delfromSubscenes);
else
subs = [];
for (i = 0; i < deletes.length; i++) {
for (j = 0; j < subs.length; j++) {
self.delFromSubscene(deletes[i], subs[j]);
}
delete self.scene.objects[deletes[i]];
}
}
if (typeof objs !== "undefined") {
Object.keys(objs).forEach(function(key){
key = parseInt(key, 10);
self.scene.objects[key] = objs[key];
self.initObj(key);
var obj = self.getObj(key),
subs = [].concat(obj.inSubscenes), k;
allsubs = allsubs.concat(subs);
for (k = 0; k < subs.length; k++)
self.addToSubscene(key, subs[k]);
});
}
if (typeof mat !== "undefined") {
self.scene.material = mat;
}
if (typeof root !== "undefined") {
self.scene.rootSubscene = root;
}
if (typeof initSubs !== "undefined")
allsubs = allsubs.concat(initSubs);
allsubs = self.unique(allsubs);
for (i = 0; i < allsubs.length; i++) {
self.initSubscene(allsubs[i]);
}
if (typeof skipRedraw !== "undefined") {
root = self.getObj(self.scene.rootSubscene);
root.par3d.skipRedraw = skipRedraw;
}
if (redraw)
self.drawScene();
};
}).call(rglwidgetClass.prototype);
rgltimerClass = function(Tick, startTime, interval, stopTime, value, rate, loop, actions) {
this.enabled = false;
this.timerId = 0;
this.startTime = startTime;         /* nominal start time in seconds */
this.value = value;                 /* current nominal time */
this.interval = interval;           /* seconds between updates */
this.stopTime = stopTime;           /* nominal stop time */
this.rate = rate;                   /* nominal units per second */
this.loop = loop;                   /* "none", "cycle", or "oscillate" */
this.realStart = undefined;         /* real world start time */
this.multiplier = 1;                /* multiplier for fast-forward
or reverse */
this.actions = actions;
this.Tick = Tick;
};
(function() {
this.play = function() {
if (this.enabled) {
this.enabled = false;
window.clearInterval(this.timerId);
this.timerId = 0;
return;
}
var tick = function(self) {
var now = new Date();
self.value = self.multiplier*self.rate*(now - self.realStart)/1000 + self.startTime;
if (self.value > self.stopTime || self.value < self.startTime) {
if (!self.loop) {
self.reset();
} else {
var cycle = self.stopTime - self.startTime,
newval = (self.value - self.startTime) % cycle + self.startTime;
if (newval < self.startTime) {
newval += cycle;
}
self.realStart += (self.value - newval)*1000/self.multiplier/self.rate;
self.value = newval;
}
}
if (typeof self.Tick !== "undefined") {
self.Tick(self.value);
}
};
this.realStart = new Date() - 1000*(this.value - this.startTime)/this.rate/this.multiplier;
this.timerId = window.setInterval(tick, 1000*this.interval, this);
this.enabled = true;
};
this.reset = function() {
this.value = this.startTime;
this.newmultiplier(1);
if (typeof this.Tick !== "undefined") {
this.Tick(this.value);
}
if (this.enabled)
this.play();  /* really pause... */
if (typeof this.PlayButton !== "undefined")
this.PlayButton.value = "Play";
};
this.faster = function() {
this.newmultiplier(Math.SQRT2*this.multiplier);
};
this.slower = function() {
this.newmultiplier(this.multiplier/Math.SQRT2);
};
this.reverse = function() {
this.newmultiplier(-this.multiplier);
};
this.newmultiplier = function(newmult) {
if (newmult != this.multiplier) {
this.realStart += 1000*(this.value - this.startTime)/this.rate*(1/this.multiplier - 1/newmult);
this.multiplier = newmult;
}
};
}).call(rgltimerClass.prototype);</script>
<div id="plot3ddiv" class="rglWebGL"></div>
<script type="text/javascript">
var plot3ddiv = document.getElementById("plot3ddiv"),
plot3drgl = new rglwidgetClass();
plot3ddiv.width = 505;
plot3ddiv.height = 505;
plot3drgl.initialize(plot3ddiv,
{"material":{"color":"#000000","alpha":1,"lit":true,"ambient":"#000000","specular":"#FFFFFF","emission":"#000000","shininess":50,"smooth":true,"front":"filled","back":"filled","size":3,"lwd":1,"fog":false,"point_antialias":false,"line_antialias":false,"texture":null,"textype":"rgb","texmipmap":false,"texminfilter":"linear","texmagfilter":"linear","texenvmap":false,"depth_mask":true,"depth_test":"less"},"rootSubscene":1,"objects":{"7":{"id":7,"type":"points","material":{"lit":false,"size":6},"vertices":[[0.270956,0.8124059,0.3277082],[0.339728,0.2883194,0.5286509],[0.08181652,0.628873,0.5658761],[0.9339687,0.4099309,0.1642652],[0.1971561,0.486255,0.832113],[0.3972987,0.7596166,0.1373741],[0.06138349,0.979789,0.7053233],[0.5910634,0.8929694,0.2876328],[0.9935346,0.08516574,0.2311903],[0.1170599,0.03371027,0.9299015],[0.4543464,0.2396767,0.01691759],[0.05794481,0.8287928,0.1203052],[0.1959302,0.1511349,0.05566179],[0.6181989,0.3709324,0.1220786],[0.01869027,0.8832075,0.04363501],[0.8550341,0.03081977,0.4749289],[0.120289,0.5732782,0.7439622],[0.2373666,0.9121111,0.114155],[0.01210717,0.6281955,0.02453074],[0.2433405,0.964567,0.1200711],[0.5251545,0.6552764,0.3485361],[0.1238965,0.4613462,0.2290099],[0.5772141,0.9640084,0.1510168],[0.9880533,0.8368791,0.8820018],[0.8607176,0.4635096,0.05747513],[0.2719098,0.5916616,0.8150284],[0.4459353,0.4131556,0.05966042],[0.1127767,0.002448122,0.2124518],[0.1898928,0.1660431,0.3602627],[0.2611665,0.4287802,0.3922152],[0.2517463,0.1570481,0.05633202],[0.1090992,0.1783884,0.4360036],[0.6355528,0.1116605,0.0698573],[0.008809316,0.9173065,0.3015817],[0.5332641,0.3693376,0.2087455],[0.9418411,0.5749845,0.909635],[0.9297088,0.2036425,0.6342672],[0.7937324,0.6349941,0.05023822],[0.5211098,0.7621584,0.6722742],[0.626762,0.1481719,0.139433],[0.2448719,0.4029988,0.8235071],[0.646358,0.5523618,0.1392542],[0.6887808,0.9741755,0.03411647],[0.848675,0.2138641,0.5287745],[0.6280065,0.7061405,0.5598994],[0.8532525,0.252234,0.5204226],[0.08847342,0.2737242,0.3841813],[0.3631095,0.5684832,0.7240314],[0.04832733,0.9772319,0.8912181],[0.7970614,0.2009498,0.5014495],[0.1899122,0.2285264,0.1639436],[0.7965345,0.188572,0.1836517],[0.9293551,0.1404706,0.1490238],[0.1318114,0.3068376,0.1083852],[0.5199742,0.3508246,0.6742347],[0.8508574,0.9175735,0.7092849],[0.2256492,0.1005429,0.04702054],[0.6812914,0.3073234,0.2815548],[0.4469795,0.609965,0.2889972],[0.2023303,0.1331885,0.5153117],[0.8312724,0.1196594,0.9719169],[0.9931236,0.6066545,0.3832877],[0.3104789,0.3440695,0.17443],[0.1074452,0.9031231,0.6939931],[0.7439377,0.2396454,0.3976088],[0.0279117,0.2219495,0.1536481],[0.8498195,0.4052949,0.7277521],[0.7973698,0.1789726,0.4774815],[0.7869422,0.4246944,0.8483545],[0.4688047,0.7431819,0.7421387],[0.582478,0.8688849,0.1669268],[0.3714654,0.5447046,0.1681659],[0.8928334,0.7836859,0.5958081],[0.005447067,0.3871703,0.4022754],[0.9728056,0.6824088,0.03188233],[0.7655979,0.6702726,0.5406112],[0.4986714,0.8169099,0.1614092],[0.2495534,0.6878175,0.5277274],[0.8643349,0.179031,0.1360152],[0.1524649,0.9447957,0.7614663],[0.877284,0.03631972,0.5483971],[0.225913,0.9063892,0.02861712],[0.1231631,0.1886397,0.5377533],[0.2426627,0.2580838,0.4619063],[0.07598623,0.1054296,0.3756129],[0.1260295,0.09798093,0.3925755],[0.9163371,0.6857722,0.8698512],[0.7639276,0.0175238,0.02267212],[0.8757943,0.6519427,0.0531094],[0.791319,0.2015895,0.3435059],[0.2150513,0.7069254,0.01822923],[0.2168552,0.8540304,0.1415181],[0.6369934,0.5504565,0.278185],[0.527912,0.6958793,0.2358698],[0.3649576,0.1300396,0.9276274],[0.05525132,0.2768639,0.4873848],[0.3897901,0.04756851,0.5482175],[0.4181431,0.2814613,0.6438522],[0.8385223,0.1376474,0.4273489],[0.5719252,0.5508786,0.5046999],[0.623111,0.05819936,0.456885],[0.7496884,0.2501413,0.4528331],[0.5399616,0.1563128,0.7111795],[0.1667123,0.1390772,0.4656253],[0.572724,0.08634555,0.7514718],[0.4245161,0.9564748,0.04470663],[0.7438336,0.400869,0.6357488],[0.6799436,0.7779377,0.5568159],[0.4986688,0.391234,0.8240591],[0.5665962,0.6644925,0.8277901],[0.9891843,0.02422757,0.07607652],[0.2478636,0.1096302,0.3092406],[0.5543849,0.7853202,0.8455557],[0.2243842,0.5438006,0.3588405],[0.9335379,0.2556463,0.1161516],[0.6674505,0.2834468,0.5836302],[0.009319689,0.7733302,0.9489557],[0.7916737,0.4490129,0.6510177],[0.268399,0.9584382,0.2235461],[0.8144639,0.8104439,0.5269571],[0.6101261,0.4381012,0.746731],[0.2032254,0.6890881,0.09712645],[0.2691386,0.8421859,0.1605125],[0.7893857,0.1417643,0.445704],[0.6781801,0.265845,0.5387418],[0.1038162,0.3408839,0.4974425],[0.664629,0.0920229,0.9443994],[0.3090551,0.04236587,0.3870697],[0.7083016,0.2741185,0.9216856],[0.3294093,0.4183654,0.3136677],[0.7104445,0.1157977,0.9679237],[0.8206233,0.7559721,0.03410616],[0.8201889,0.6455923,0.9404398],[0.4589804,0.933157,0.5429799],[0.255881,0.9785376,0.07881562],[0.8226316,0.6593776,0.4061187],[0.884883,0.68674,0.05447629],[0.5366461,0.2407877,0.6126193],[0.611062,0.5016919,0.3286982],[0.8415577,0.7718162,0.2397421],[0.17941,0.4686844,0.7793702],[0.6843942,0.06192318,0.9862406],[0.8435186,0.2535193,0.05269436],[0.7870569,0.02535332,0.5262576],[0.6997044,0.8687286,0.3190735],[0.6086001,0.9150085,0.3854055],[0.3999627,0.2442574,0.5599252],[0.3753399,0.4284283,0.3742581],[0.198584,0.7512906,0.1406495],[0.9157157,0.3641839,0.8143049],[0.0316989,0.8104318,0.9444556],[0.5083786,0.948253,0.3999255],[0.1833674,0.7938512,0.3463233],[0.3197395,0.1293094,0.3771501],[0.4707104,0.1359682,0.7553539],[0.1549076,0.9384621,0.2641518],[0.9870385,0.9589605,0.5924452],[0.05987407,0.1461096,0.1107785],[0.1143497,0.1156997,0.9811177],[0.3785148,0.2919713,0.3559611],[0.6745352,0.4611044,0.255742],[0.3872116,0.965178,0.1575038],[0.08803946,0.6762688,0.09020875],[0.694334,0.9503214,0.3060817],[0.1543014,0.5670496,0.6996848],[0.123203,0.3606174,0.2490901],[0.08384347,0.7520081,0.3150007],[0.1383442,0.3821934,0.9778967],[0.4853702,0.3948451,0.6003619],[0.5011882,0.2581383,0.1030055],[0.5925961,0.02698919,0.08947034],[0.9774102,0.4153953,0.5731856],[0.8646322,0.5695168,0.3245082],[0.2240613,0.3363567,0.5498168],[0.8997468,0.01293074,0.9946056],[0.9026006,0.8156065,0.1293845],[0.6032562,0.60717,0.2332968],[0.616497,0.05272305,0.3242261],[0.7713637,0.9214613,0.8151299],[0.4437077,0.3137707,0.6940348],[0.112856,0.6741397,0.1292429],[0.09028084,0.8570399,0.9715905],[0.6894628,0.1585298,0.5844234],[0.07077761,0.715845,0.5674541],[0.3859416,0.8388212,0.01025186],[0.8812929,0.886176,0.3269034],[0.2828522,0.146271,0.9053653],[0.8811206,0.4889778,0.1287211],[0.8184969,0.7288231,0.9817606],[0.027677,0.794619,0.9769028],[0.5088043,0.3949464,0.6567281],[0.2152073,0.5813779,0.34922],[0.479081,0.7922525,0.259438],[0.8514454,0.3076966,0.932013],[0.2247722,0.003574531,0.1740142],[0.8357224,0.7038785,0.4693035],[0.2850866,0.05084247,0.1320105],[0.6203282,0.6799995,0.4052313],[0.8417121,0.6740907,0.3908655],[0.3928452,0.7007608,0.9481952],[0.5062841,0.9820394,0.2822124],[0.6802782,0.2851242,0.04239904],[0.6229806,0.6002409,0.2189276],[0.4151322,0.7045116,0.3420456],[0.5426524,0.954741,0.2209489],[0.004468052,0.5139679,0.7027399],[0.5778334,0.3606974,0.7347071],[0.7107031,0.706886,0.5632248],[0.3672411,0.1446274,0.4467101],[0.8362501,0.5756233,0.2872394],[0.4289004,0.6360493,0.3256666],[0.1435122,0.2541205,0.09037109],[0.4805129,0.5699418,0.2637929],[0.6599458,0.3337511,0.2368524],[0.3598008,0.6797889,0.08356781],[0.5135612,0.7384623,0.7688617],[0.8921538,0.2885388,0.5072011],[0.9515328,0.109326,0.8990846],[0.2952311,0.3356393,0.009501632],[0.04178278,0.9592957,0.0239505],[0.05750178,0.13323,0.5098522],[0.2385067,0.02077828,0.3995838],[0.7200331,0.4379356,0.7610236],[0.06517167,0.3413539,0.405179],[0.5253382,0.253823,0.04662498],[0.1345057,0.956517,0.8202046],[0.06359477,0.8765337,0.2259262],[0.642313,0.891848,0.04286834],[0.8368759,0.360198,0.3598628],[0.352295,0.6917877,0.5472175],[0.231095,0.7919766,0.9036624],[0.2788297,0.8575465,0.5999774],[0.778231,0.4740622,0.0890776],[0.9086965,0.5266357,0.4221227],[0.2345455,0.8404368,0.9811774],[0.5407688,0.07880231,0.3007811],[0.6906641,0.9877006,0.6104504],[0.02964015,0.5655652,0.0410066],[0.7185504,0.004326933,0.4153787],[0.8940973,0.4204474,0.7928818],[0.3788327,0.85595,0.4076351],[0.9707994,0.0264636,0.7713273],[0.2759047,0.3128908,0.07674231],[0.4777076,0.2379093,0.00425959],[0.8107567,0.5843107,0.2172831],[0.7526672,0.104906,0.1259282],[0.5821646,0.1091026,0.0558827],[0.9745858,0.4210176,0.9896051],[0.0615777,0.3141613,0.452683],[0.5917662,0.4066314,0.4131185],[0.3087108,0.2949614,0.4038092],[0.2624937,0.8892192,0.5878615],[0.7664469,0.434385,0.6421778],[0.7729462,0.0911948,0.5011417],[0.8718159,0.3378896,0.2723646],[0.1232033,0.8079789,0.435391],[0.3167284,0.2059089,0.2353772],[0.2790607,0.3885424,0.5334129],[0.1759923,0.5585449,0.617483],[0.882231,0.3817382,0.3172487],[0.04501222,0.1778622,0.1078495],[0.725508,0.5737641,0.6266844],[0.8856388,0.3125403,0.2408604],[0.1675298,0.7229896,0.09564218],[0.2532063,0.2092023,0.351648],[0.3461677,0.3725057,0.373862],[0.8393751,0.8371015,0.5536796],[0.701614,0.9455406,0.6538313],[0.2219456,0.8082437,0.5489432],[0.7579771,0.2859723,0.05701023],[0.5230885,0.2951223,0.169255],[0.02707036,0.3342298,0.1747278],[0.5568224,0.5470837,0.05717333],[0.4209177,0.3591011,0.8792979],[0.7025234,0.1061665,0.5291857],[0.07505986,0.3472486,0.3606921],[0.6930292,0.2914793,0.3678991],[0.9500815,0.432723,0.3690906],[0.9003539,0.08086746,0.6286795],[0.6624048,0.3733809,0.1617432],[0.05530744,0.4360713,0.1489772],[0.04214885,0.01389125,0.4928742],[0.6240939,0.02134971,0.1679796],[0.1893312,0.1569691,0.4246679],[0.9524087,0.5174455,0.1697397],[0.6907473,0.7563787,0.5915025],[0.1219934,0.5714145,0.2787178],[0.4211693,0.8649276,0.6771061],[0.2389591,0.7028099,0.3799395],[0.9075295,0.8982301,0.1479805],[0.3368321,0.8242054,0.5950226],[0.7972099,0.9399766,0.3524249],[0.5967492,0.2357857,0.7605378],[0.03000334,0.04540993,0.2442439],[0.8722068,0.5711665,0.7396023],[0.9756932,0.3628621,0.2341644],[0.7067322,0.386458,0.041742],[0.7722691,0.4460253,0.1486393],[0.8342686,0.194949,0.3475249],[0.06885353,0.8583329,0.04042793],[0.5531633,0.4956573,0.9828901],[0.9807071,0.7829766,0.1314494],[0.1751627,0.2008764,0.1939813],[0.2895544,0.3162114,0.1179191],[0.4146753,0.119982,0.8552463],[0.9662099,0.6908314,0.7164552],[0.8907881,0.2099846,0.01057623],[0.04984891,0.2633872,0.3724208],[0.4795951,0.2403383,0.7062563],[0.3811385,0.3841362,0.149194],[0.111769,0.5501095,0.8697262],[0.912301,0.769827,0.860171],[0.5610743,0.04269535,0.3728599],[0.8975698,0.1836712,0.3581578],[0.117804,0.263405,0.4836182],[0.04164989,0.8549811,0.5510612],[0.1842253,0.7771528,0.1633653],[0.4661035,0.4001307,0.1991804],[0.2363108,0.2591196,0.4711879],[0.4326396,0.2343428,0.6965451],[0.06664436,0.5174368,0.7802794],[0.4644529,0.930394,0.01971098],[0.9792625,0.5912515,0.3968413],[0.6982156,0.3180582,0.9363747],[0.9385859,0.9019738,0.3346283],[0.6715633,0.7468174,0.9785843],[0.7041028,0.70008,0.2009973],[0.7416335,0.726841,0.4479916],[0.002022203,0.1302251,0.9399925],[0.1612432,0.5808803,0.8541881],[0.6404414,0.7396501,0.4785343],[0.999822,0.9062732,0.9871581],[0.9062259,0.2510424,0.8695824],[0.8678694,0.1894719,0.5635728],[0.8610322,0.4158821,0.1527234],[0.8036636,0.5835308,0.8577595],[0.7783054,0.9699059,0.2338534],[0.6268761,0.8689761,0.1519084],[0.5498987,0.3327376,0.7330548],[0.08958354,0.6465266,0.9241467],[0.03200383,0.02838162,0.3502955],[0.9080837,0.1185535,0.1108892],[0.6012642,0.8866001,0.3656538],[0.9950901,0.6949722,0.6088338],[0.3498011,0.003132959,0.1463962],[0.4872715,0.3005398,0.1940881],[0.3353269,0.5341323,0.6461917],[0.7527327,0.5855935,0.8958412],[0.1404653,0.001620509,0.9785998],[0.1098449,0.771822,0.4669435],[0.4803346,0.8654415,0.2939362],[0.1258991,0.327092,0.368734],[0.965899,0.5626671,0.5885755],[0.9339,0.1560485,0.8919647],[0.4055457,0.6599495,0.1437558],[0.2854518,0.67629,0.1360819],[0.1468085,0.5452735,0.02897464],[0.3803596,0.509112,0.3691459],[0.7787759,0.09336269,0.5157034],[0.2315899,0.7395467,0.1097114],[0.8268521,0.4212603,0.0003576479],[0.9011801,0.1522511,0.8484592],[0.9805238,0.4711404,0.5926341],[0.6575539,0.2636992,0.1319096],[0.9132458,0.5339597,0.2286678],[0.2348486,0.4041704,0.3174894],[0.6831527,0.9886117,0.6638672],[0.1519558,0.5775766,0.8522779],[0.573355,0.7585201,0.2508304],[0.8083931,0.5833808,0.197785],[0.2011301,0.06672417,0.3792828],[0.5630933,0.0509206,0.6412811],[0.1093802,0.6612357,0.5692042],[0.9408494,0.5510031,0.2057537],[0.8581802,0.09596458,0.5532867],[0.6887821,0.7320033,0.2826708],[0.4722558,0.9219806,0.04105844],[0.4533991,0.4511177,0.9420172],[0.5472632,0.9954231,0.08556546],[0.9712889,0.3020952,0.003584702],[0.07816486,0.7307197,0.9347373],[0.3703812,0.6651318,0.3351983],[0.8054785,0.6103267,0.5350451],[0.2200541,0.7059799,0.133855],[0.8818889,0.9740203,0.767288],[0.8859101,0.5076312,0.7979087],[0.02289913,0.5813037,0.7037878],[0.5912546,0.6328166,0.3627546],[0.7123618,0.9450372,0.3742678],[0.6317844,0.8254454,0.3809356],[0.3141735,0.8063547,0.1043045],[0.008777333,0.003269027,0.5460063],[0.777505,0.09686333,0.5802317],[0.02214603,0.4865232,0.8008839],[0.7457403,0.0931792,0.9556206],[0.3348725,0.1173491,0.1438058],[0.8362657,0.2287827,0.9861339],[0.9354731,0.6706001,0.6756568],[0.5994512,0.3760856,0.8329521],[0.002998041,0.9914191,0.2714364],[0.8129847,0.5523942,0.09996776],[0.4874673,0.07329261,0.2637025],[0.1898265,0.861712,0.158208],[0.1712121,0.6230633,0.4441638],[0.6990395,0.6492897,0.2239211],[0.04484119,0.5214429,0.8816596],[0.3284501,0.4892816,0.8928865],[0.2003263,0.9238617,0.3896406],[0.9464346,0.8365219,0.8709311],[0.7375295,0.4704418,0.07604808],[0.01586564,0.5000671,0.05018213],[0.07870673,0.304803,0.1008227],[0.4282252,0.07355248,0.02314295],[0.8554245,0.5998793,0.7320669],[0.6097917,0.6110688,0.9800761],[0.6385395,0.5189751,0.8112603],[0.9755207,0.9094111,0.1019356],[0.6978943,0.001212666,0.04578892],[0.7642244,0.9940141,0.06573977],[0.1352308,0.1226549,0.516371],[0.4952544,0.05013798,0.1398177],[0.7007978,0.804586,0.1571259],[0.570461,0.9054978,0.02374578],[0.5388666,0.2998436,0.6164582],[0.5075229,0.02585634,0.7500738],[0.2748364,0.5226046,0.2267591],[0.6479139,0.3964883,0.5104172],[0.1093687,0.4789625,0.263835],[0.6064209,0.9953534,0.6703193],[0.6407182,0.72223,0.2107718],[0.1696519,0.7764432,0.1996092],[0.4103282,0.9034811,0.4853603],[0.4523315,0.7923985,0.3688036],[0.4571875,0.1600944,0.1294079],[0.9440072,0.7277213,0.2534567],[0.4843549,0.9145653,0.9989919],[0.9871989,0.3132344,0.2738053],[0.18587,0.7123095,0.1477248],[0.3603475,0.219793,0.09760268],[0.6468731,0.02346477,0.9097803],[0.6152851,0.3352257,0.3801033],[0.7075157,0.5122435,0.8389233],[0.3296887,0.8466026,0.2182844],[0.01622046,0.3989755,0.447916],[0.4141531,0.9156489,0.1470558],[0.1891999,0.279223,0.9558859],[0.4385836,0.2450408,0.3436106],[0.5085086,0.3882283,0.002141457],[0.7132118,0.3095436,0.2606763],[0.1025925,0.46432,0.1030474],[0.6554641,0.9654243,0.3732085],[0.1229307,0.07058309,0.2559245],[0.3718702,0.4446562,0.1254205],[0.8693256,0.1085515,0.9069666],[0.8004019,0.9556031,0.7198588],[0.6082353,0.720969,0.3135169],[0.963805,0.2803282,0.674181],[0.1955235,0.8735915,0.4319008],[0.7203729,0.2643699,0.1021005],[0.4107635,0.03008889,0.3430972],[0.5221941,0.9567546,0.2919371],[0.8189524,0.03638248,0.2594411],[0.02299816,0.07369407,0.07907515],[0.2683749,0.9121143,0.8377782],[0.5573077,0.8716689,0.6977855],[0.09890632,0.7957814,0.09542608],[0.7417747,0.2041998,0.02397048],[0.6134654,0.8849981,0.3410067],[0.6745008,0.6485907,0.4434016],[0.7633588,0.8364934,0.4227943],[0.8789722,0.0813617,0.3712786],[0.2988829,0.7809265,0.1782501],[0.9798177,0.6977059,0.845987],[0.3978385,0.7601932,0.503835],[0.3140472,0.1506542,0.5273804],[0.7382069,0.8002579,0.9920936],[0.9915762,0.3440266,0.05251123],[0.7971563,0.6627979,0.9597374],[0.2456043,0.4078822,0.8286775],[0.5143243,0.6750889,0.3680185],[0.8418086,0.8047842,0.2013299],[0.4166574,0.887156,0.7137542],[0.4074057,0.5906084,0.2218859],[0.05991314,0.5118791,0.8484272],[0.7602676,0.2355108,0.2161297],[0.4569128,0.3954383,0.1203131],[0.6230573,0.1919032,0.9503594],[0.4022436,0.4494502,0.1431572],[0.6989695,0.7132365,0.504388],[0.9924118,0.9828471,0.03988377],[0.6635498,0.4363963,0.6033368],[0.8426574,0.09320974,0.9949651],[0.4847158,0.005049093,0.6591319],[0.7422459,0.8505942,0.02040913],[0.5679761,0.9305258,0.4349755],[0.01658792,0.04501205,0.09195523],[0.3914092,0.4285314,0.738937],[0.7795367,0.2839584,0.3707983],[0.5652453,0.08891171,0.1728705],[0.3947785,0.8366242,0.1825859],[0.7945364,0.5992881,0.881057],[0.05081595,0.596207,0.129341],[0.04327673,0.04491724,0.07552645],[0.9524975,0.7901428,0.1199638],[0.1431417,0.01537926,0.9001122],[0.3374082,0.9591017,0.6834795],[0.8764121,0.9050407,0.3116206],[0.9044642,0.3288503,0.7552186],[0.7554722,0.9211066,0.8131778],[0.2868456,0.4053591,0.08700886],[0.1400089,0.4016947,0.8089546],[0.8582714,0.3490339,0.1250552],[0.2378043,0.7342828,0.4600169],[0.1303854,0.832009,0.2295302],[0.1825986,0.001949112,0.4261869],[0.7265909,0.4679423,0.1073152],[0.2391569,0.3549084,0.4335623],[0.9078784,0.6663331,0.6927013],[0.8612657,0.6922292,0.9496523],[0.171904,0.5349746,0.6941084],[0.1649713,0.5658841,0.01678043],[0.6932481,0.1694014,0.1973547],[0.5926282,0.5339214,0.9945816],[0.09486501,0.6528681,0.3718326],[0.8738722,0.614916,0.5892155],[0.8643004,0.3571738,0.3129403],[0.7438174,0.9019154,0.1663421],[0.1448396,0.62573,0.3325768],[0.1204262,0.4577306,0.8728039],[0.9318317,0.7358863,0.3259563],[0.08359949,0.554901,0.1966319],[0.1804264,0.6768677,0.05344877],[0.5304279,0.1970111,0.7497321],[0.4710418,0.5868275,0.9896713],[0.4309578,0.4248174,0.05460518],[0.7968376,0.4358988,0.1529393],[0.7285018,0.3026343,0.5080059],[0.5947081,0.2404894,0.3101921],[0.6666261,0.3940044,0.006250999],[0.1504667,0.6594449,0.965236],[0.3908215,0.8778233,0.07324158],[0.8748699,0.03757825,0.0146193],[0.5057289,0.8685477,0.1248035],[0.9812524,0.7533667,0.8296431],[0.4462835,0.3950073,0.8750625],[0.01962186,0.3738714,0.4872614],[0.0766587,0.6520039,0.03965764],[0.3151932,0.7580474,0.1113485],[0.2467767,0.8792401,0.4678466],[0.6606621,0.8600206,0.009000621],[0.2023306,0.4665552,0.6450259],[0.8981475,0.0921994,0.52089],[0.2635963,0.4124482,0.1107278],[0.979224,0.6802778,0.1235441],[0.7876536,0.1323127,0.1553956],[0.8347445,0.6217793,0.2941952],[0.09720937,0.1213271,0.9164401],[0.7073036,0.9898723,0.2077476],[0.6836618,0.8800549,0.1410724],[0.7565687,0.09807366,0.463263],[0.134476,0.9315296,0.3552936],[0.1841529,0.2810341,0.5339861],[0.3388878,0.6033257,0.5119682],[0.5689567,0.444641,0.4024922],[0.9514536,0.9153034,0.02077597],[0.05764429,0.7737213,0.9503134],[0.9470703,0.2428845,0.05095523],[0.5409303,0.3650789,0.3346318],[0.4896387,0.8362569,0.6129729],[0.8617996,0.2687113,0.5497404],[0.1354447,0.1963761,0.3715279],[0.3757158,0.5003306,0.2122137],[0.8133289,0.720393,0.4559765],[0.3746369,0.3463143,0.3368754],[0.2781214,0.7662342,0.4778734],[0.5419187,0.8152454,0.1859433],[0.9848443,0.9865587,0.4246545],[0.9831887,0.6903068,0.7106765],[0.6630965,0.5746457,0.10702],[0.1007329,0.3426162,0.4718232],[0.4885853,0.7765095,0.07353193],[0.646936,0.7505845,0.2397815],[0.1787546,0.8185402,0.2911724],[0.6227978,0.8921314,0.1839604],[0.01757807,0.1994732,0.2629862],[0.7983946,0.2806315,0.5997507],[0.3948658,0.2212407,0.3868829],[0.3003035,0.6980917,0.5264721],[0.1649283,0.8016341,0.2482744],[0.8608962,0.7351644,0.5431377],[0.5948412,0.6048048,0.6536132],[0.02427213,0.7231531,0.06860469],[0.2616418,0.2629994,0.2793049],[0.8095182,0.5187546,0.8006856],[0.1055581,0.6866038,0.06717064],[0.9736134,0.1781756,0.2119161],[0.9587163,0.9114941,0.3695781],[0.2770989,0.3896606,0.1611973],[0.6998586,0.4369403,0.858333],[0.7878158,0.5235511,0.8045433],[0.9373181,0.329714,0.7221518],[0.04410455,0.9518002,0.6222757],[0.7577251,0.7987091,0.03096572],[0.68283,0.6665297,0.3972919],[0.8648934,0.6985518,0.4023553],[0.9406583,0.1709769,0.708323],[0.3225898,0.9710565,0.8025261],[0.07730129,0.899613,0.2910788],[0.8194441,0.2173542,0.1348961],[0.1594302,0.8736352,0.07612028],[0.968281,0.3239083,0.819782],[0.8425965,0.2700415,0.1762823],[0.3427286,0.919786,0.6100376],[0.543712,0.4287878,0.2886489],[0.3881812,0.5434511,0.2811292],[0.06098921,0.2613012,0.1068534],[0.3129643,0.05075523,0.5904489],[0.2095001,0.9457818,0.08816009],[0.5057392,0.3639237,0.1217383],[0.5060917,0.9433605,0.5588754],[0.3515147,0.9497387,0.01079994],[0.09952402,0.2317563,0.5218825],[0.09660386,0.5726692,0.8116875],[0.1046332,0.1568016,0.9802425],[0.6502258,0.1320816,0.3140722],[0.4417999,0.223363,0.3220521],[0.4050542,0.7049772,0.1310506],[0.3830844,0.5693383,0.2807615],[0.6404404,0.3056727,0.5635602],[0.8828412,0.8139481,0.4066896],[0.9154798,0.2631308,0.6300376],[0.9086897,0.4638479,0.03578534],[0.891431,0.2135609,0.2860225],[0.5457854,0.4864144,0.193337],[0.2935387,0.3401692,0.0231246],[0.8014858,0.157885,0.1182548],[0.6968247,0.2262529,0.9901936],[0.9049159,0.8524192,0.8866068],[0.7861432,0.7156464,0.9634863],[0.8337231,0.3168902,0.4364631],[0.3479899,0.2541869,0.3150941],[0.8265161,0.7304065,0.3738916],[0.4277714,0.1100659,0.05255983],[0.3791581,0.5816537,0.1383352],[0.7018435,0.2629349,0.140754],[0.222402,0.1892026,0.995185],[0.2100355,0.2135195,0.549095],[0.5471898,0.046568,0.8336387],[0.4574795,0.2949133,0.7024872],[0.7661688,0.6295558,0.4799606],[0.9400573,0.2273693,0.135815],[0.143177,0.9681278,0.6209091],[0.633462,0.895883,0.9516718],[0.8715613,0.06579074,0.3514523],[0.6019812,0.8159906,0.5045606],[0.2419881,0.4188913,0.3992342],[0.3848269,0.5627536,0.3083453],[0.6222898,0.7667878,0.2297547],[0.3713199,0.5896947,0.0219754],[0.3661808,0.3394423,0.513129],[0.2041376,0.9087263,0.7101779],[0.9473315,0.6478429,0.7009748],[0.5777516,0.7774305,0.05453363],[0.06254588,0.09253263,0.1699014],[0.350758,0.961531,0.8101431],[0.7124114,0.6325803,0.2401294],[0.3507365,0.3906885,0.01287977],[0.7921059,0.4493259,0.750712],[0.1058607,0.6252477,0.02632205],[0.6196839,0.7148029,0.2986535],[0.7651018,0.1194663,0.4965323],[0.4764369,0.771054,0.8827561],[0.3372615,0.725863,0.97885],[0.7811131,0.03463888,0.06058018],[0.7622159,0.3234169,0.474398],[0.8897331,0.2131212,0.3201732],[0.368294,0.6084016,0.5890709],[0.5533566,0.09070392,0.1685765],[0.1892062,0.05120347,0.5386987],[0.7452682,0.9078513,0.2844285],[0.1292213,0.07813044,0.3342875],[0.1885016,0.2731214,0.09862859],[0.1954716,0.0977504,0.5624629],[0.7476602,0.8560746,0.5714936],[0.606062,0.2516818,0.0007076142],[0.4290569,0.4163084,0.4299097],[0.877269,0.6651017,0.4326306],[0.6130383,0.09655346,0.5300621],[0.1010395,0.2559853,0.3572788],[0.3859218,0.6092653,0.9853331],[0.5963485,0.7678604,0.1495827],[0.1452219,0.7011864,0.9887306],[0.1399227,0.1095112,0.4218689],[0.2954761,0.1863925,0.09948967],[0.3904811,0.9414756,0.6706014],[0.4921306,0.7043031,0.6322739],[0.7764632,0.05353354,0.9454409],[0.5674757,0.9458677,0.5936499],[0.6589653,0.5428843,0.7197235],[0.142778,0.6249906,0.4310954],[0.6478862,0.7520018,0.5550422],[0.5753781,0.6428936,0.1149073],[0.9110243,0.2922144,0.1658856],[0.6148176,0.7113149,0.4056941],[0.2916553,0.6025245,0.9742132],[0.6379832,0.7964326,0.01347817],[0.5349647,0.06005703,0.3726864],[0.5710453,0.6777078,0.2921865],[0.714864,0.5560918,0.7496186],[0.4281344,0.8229896,0.2544633],[0.3683299,0.4151402,0.7521948],[0.59443,0.4310326,0.01528303],[0.3830533,0.5829638,0.2710248],[0.1197591,0.1149003,0.2581219],[0.6924039,0.5545745,0.8257744],[0.3303028,0.8694375,0.3012603],[0.7436187,0.6822206,0.5655456],[0.7100012,0.5763988,0.8931471],[0.9529852,0.192776,0.1981504],[0.1883372,0.3837759,0.5574169],[0.7120694,0.03149105,0.4303229],[0.3630335,0.9936557,0.7765989],[0.05634804,0.4488167,0.7905964],[0.8402082,0.883473,0.208261],[0.8793039,0.8442072,0.4626269],[0.212134,0.7337624,0.2975561],[0.7632441,0.8155888,0.9967182],[0.5527397,0.6366978,0.7384346],[0.7328045,0.3194709,0.2271044],[0.8550742,0.6148975,0.4074123],[0.6056051,0.6388928,0.9515616],[0.2335246,0.8718236,0.1529955],[0.2421332,0.5774788,0.3319368],[0.04664214,0.1018921,0.9548021],[0.9445244,0.6202074,0.1430607],[0.2064646,0.4069621,0.8517151],[0.7282557,0.8794106,0.3471794],[0.4043675,0.7485863,0.05210817],[0.09487712,0.8624592,0.3294469],[0.07201727,0.9387043,0.07072176],[0.1234582,0.142942,0.03121849],[0.7367825,0.3146619,0.2866315],[0.8691689,0.1306546,0.9173884],[0.6594132,0.6006732,0.435275],[0.4425798,0.9401539,0.2808141],[0.8954968,0.7782944,0.4100751],[0.6253563,0.2389108,0.1988841],[0.9200588,0.2163982,0.1032619],[0.05658745,0.1282517,0.0823506],[0.7631497,0.5215948,0.6817681],[0.2118441,0.4423235,0.7597719],[0.6753439,0.8095644,0.5820255],[0.7932175,0.8637129,0.2894087],[0.2741465,0.3086627,0.5826185],[0.8348137,0.9070828,0.2334401],[0.1672576,0.7080984,0.4861816],[0.4996527,0.9657624,0.9993041],[0.3453745,0.9388385,0.8343057],[0.3616396,0.8562248,0.1291977],[0.01312375,0.04156399,0.5768628],[0.7018677,0.3147714,0.257033],[0.5225391,0.06393015,0.1039596],[0.1440795,0.3117091,0.05925139],[0.727734,0.2610545,0.5482287],[0.9326493,0.4941159,0.06145179],[0.9530427,0.5539443,0.9846425],[0.8579511,0.7527741,0.1213191],[0.304114,0.9759378,0.1116001],[0.4467027,0.2698409,0.1688536],[0.3047281,0.9570543,0.2300928],[0.4243942,0.7238415,0.3399117],[0.715962,0.3419712,0.2382799],[0.8185193,0.7477506,0.3903706],[0.2318523,0.2566975,0.3975715],[0.4694611,0.3346855,0.1420296],[0.1549694,0.1074988,0.4683245],[0.07745758,0.003525609,0.01572911],[0.9723916,0.2905175,0.2064929],[0.3057032,0.6285105,0.3637211],[0.4771083,0.1481287,0.7498872],[0.2733387,0.4809984,0.02659419],[0.2396526,0.9090971,0.2394431],[0.002745515,0.7428486,0.4487954],[0.4635013,0.6043754,0.06064134],[0.1450482,0.3252193,0.360779],[0.6819564,0.8977618,0.02050897],[0.5720799,0.6895346,0.1727647],[0.2647404,0.8236326,0.9596339],[0.8026654,0.4450908,0.8794767],[0.9256909,0.6965672,0.06793532],[0.2653168,0.7452053,0.2557293],[0.2370935,0.6317569,0.09692019],[0.3058413,0.7933394,0.4472271],[0.01342801,0.5187605,0.2374093],[0.1507909,0.2400866,0.5780475],[0.8923087,0.2444188,0.4090492],[0.6984617,0.2930485,0.07015125],[0.1990142,0.1360231,0.07929788],[0.2753566,0.08799621,0.04591766],[0.1835264,0.1612839,0.404529],[0.2508898,0.9802253,0.837227],[0.9766802,0.9063262,0.4804054],[0.3499265,0.06410669,0.5729151],[0.02055014,0.8283018,0.3080007],[0.312508,0.1445577,0.23998],[0.9202878,0.9284864,0.4560237],[0.01772781,0.4361217,0.08445555],[0.4646213,0.259203,0.2768611],[0.4431858,0.3894852,0.6807607],[0.39501,0.113275,0.3748507],[0.9372081,0.8286007,0.8807833],[0.01482022,0.6912701,0.5390469],[0.8627195,0.6437809,0.4999591],[0.9078777,0.5416033,0.9833603],[0.02028302,0.4892141,0.0255557],[0.6862425,0.7078261,0.2415482],[0.4444191,0.9390196,0.08189515],[0.2891088,0.6915336,0.4042208],[0.2509878,0.4044805,0.6651818],[0.6859226,0.8490096,0.3185745],[0.8523493,0.6903115,0.1262356],[0.07840697,0.06507296,0.960499],[0.4348946,0.2656779,0.03740625],[0.879352,0.3211545,0.3754799],[0.0224725,0.7470324,0.2377684],[0.8136806,0.07210518,0.4233399],[0.5542477,0.1428635,0.6864997],[0.6455499,0.8799158,0.2746847],[0.3047399,0.9089209,0.6842065],[0.04782989,0.2550043,0.1804728],[0.6274424,0.6744105,0.1943641],[0.547159,0.8432369,0.7425972],[0.1113352,0.1668285,0.4849556],[0.7655746,0.182412,0.4607148],[0.6977473,0.5206379,0.8068109],[0.3765805,0.2785719,0.5274165],[0.9234649,0.965398,0.9192127],[0.3403862,0.9826154,0.3325526],[0.5894636,0.3425842,0.02565965],[0.1932934,0.9918802,0.1722992],[0.08454818,0.02391287,0.1342129],[0.7320578,0.7197176,0.1076004],[0.894274,0.9136862,0.1133154],[0.06982326,0.8877488,0.9335943],[0.6617953,0.1059647,0.3433868],[0.04798799,0.8077517,0.158458],[0.07690156,0.7743883,0.9192176],[0.184206,0.1763955,0.9116448],[0.2876672,0.3541079,0.2188042],[0.9770598,0.2077172,0.1195427],[0.1679376,0.8824788,0.2761688],[0.9355891,0.5324804,0.3194948],[0.2299964,0.5946591,0.2297624],[0.7806079,0.01662784,0.4078861],[0.8853427,0.7296007,0.3253926],[0.9878823,0.06979974,0.6905417],[0.7418671,0.2040863,0.2273316],[0.1746745,0.8068922,0.9440175],[0.9038535,0.4522993,0.02177001],[0.2094224,0.8147113,0.4501432],[0.1259422,0.2510449,0.9124339],[0.4772216,0.2052998,0.6149561],[0.09684517,0.5564294,0.7198447],[0.973294,0.8003299,0.2328485],[0.4136997,0.3840894,0.7377988],[0.4926799,0.3556009,0.2567271],[0.07022183,0.8468416,0.4104985],[0.4661995,0.1812757,0.6998419],[0.9837285,0.3817531,0.7633581],[0.6196785,0.2102056,0.1246073],[0.7294798,0.09575338,0.0469557],[0.0634466,0.1647829,0.1671746],[0.5022808,0.9569576,0.3571503],[0.07496174,0.4601299,0.8478738],[0.3238432,0.09073412,0.1632024],[0.2058291,0.01145239,0.1735193],[0.3661427,0.5123239,0.8526506],[0.8407127,0.3083669,0.1038387],[0.6291417,0.06078131,0.3163382],[0.7119459,0.1752802,0.2122921],[0.2219913,0.8224792,0.2973896],[0.735857,0.3688692,0.04336479],[0.01741841,0.6610817,0.4831148],[0.367685,0.9177188,0.2213023],[0.7937216,0.5334988,0.2780769],[0.3824705,0.08803483,0.3719232],[0.6571894,0.07695857,0.2575436],[0.06907396,0.02305604,0.09112689],[0.1836423,0.03042321,0.5665804],[0.04558595,0.9324376,0.3008339],[0.07300608,0.8410136,0.07939438],[0.9402593,0.9560778,0.1548008],[0.499194,0.6678943,0.8584014],[0.8932449,0.4067459,0.3098028],[0.2168622,0.7813427,0.194105],[0.9845688,0.4310915,0.9534854],[0.6600198,0.7088512,0.3288195],[0.834246,0.9276955,0.1965624],[0.3305761,0.1594952,0.2271687],[0.4747568,0.8255891,0.8298818],[0.6851702,0.5940815,0.6416696],[0.2448724,0.131447,0.5076517],[0.9985381,0.2582079,0.3549165],[0.7565879,0.1383926,0.3378993],[0.8582726,0.6576093,0.4098599],[0.03078299,0.4215254,0.308689],[0.6328759,0.80744,0.1342005],[0.5524927,0.2988904,0.8847572],[0.1584617,0.8980558,0.3988017],[0.406028,0.6250166,0.8279935],[0.5595255,0.5110667,0.09634256],[0.7670653,0.8998862,0.4572381],[0.6302104,0.8922837,0.2480944],[0.7446564,0.4246858,0.3901175],[0.8499277,0.5507814,0.2177557],[0.7274488,0.4236023,0.3657607],[0.2739173,0.1679625,0.4365861],[0.3893525,0.3379947,0.9002159],[0.7324348,0.5782148,0.3077096],[0.3819163,0.7118912,0.9606726],[0.08534239,0.07271635,0.2729814],[0.3601942,0.8287733,0.2037097],[0.9983507,0.1241636,0.170898],[0.9779375,0.5441275,0.3697605],[0.2645564,0.1380209,0.1238847],[0.06583135,0.259555,0.1527514],[0.7703986,0.8449654,0.4078432],[0.672639,0.6023911,0.5838399],[0.2707793,0.9051656,0.2714971],[0.8429348,0.6448606,0.2281958],[0.9063564,0.4370343,0.5786622],[0.6166458,0.8868197,0.3941298],[0.934682,0.9796647,0.5568718],[0.1469417,0.08881273,0.3979052],[0.463529,0.924481,0.0246367],[0.3187331,0.789187,0.5363152],[0.4868439,0.371725,0.0961632],[0.9080275,0.870792,0.810964],[0.6244642,0.580241,0.04364656],[0.3037448,0.6671388,0.9676614],[0.1186313,0.9473036,0.7078985],[0.872466,0.7203887,0.5479698],[0.3503369,0.6840641,0.5190206],[0.02756294,0.9918048,0.357626],[0.3501461,0.9921781,0.1213199],[0.3773305,0.3788023,0.1761495],[0.726885,0.9149681,0.128943],[0.1770431,0.857592,0.09952889],[0.9029484,0.9184938,0.5346761],[0.02755318,0.3940175,0.1452875],[0.6211736,0.8925311,0.38829],[0.2559045,0.09446543,0.9244592],[0.5280436,0.07620411,0.05985307],[0.2278901,0.04276507,0.4910267]],"colors":[[0,0,0,1]],"centers":[[0.270956,0.8124059,0.3277082],[0.339728,0.2883194,0.5286509],[0.08181652,0.628873,0.5658761],[0.9339687,0.4099309,0.1642652],[0.1971561,0.486255,0.832113],[0.3972987,0.7596166,0.1373741],[0.06138349,0.979789,0.7053233],[0.5910634,0.8929694,0.2876328],[0.9935346,0.08516574,0.2311903],[0.1170599,0.03371027,0.9299015],[0.4543464,0.2396767,0.01691759],[0.05794481,0.8287928,0.1203052],[0.1959302,0.1511349,0.05566179],[0.6181989,0.3709324,0.1220786],[0.01869027,0.8832075,0.04363501],[0.8550341,0.03081977,0.4749289],[0.120289,0.5732782,0.7439622],[0.2373666,0.9121111,0.114155],[0.01210717,0.6281955,0.02453074],[0.2433405,0.964567,0.1200711],[0.5251545,0.6552764,0.3485361],[0.1238965,0.4613462,0.2290099],[0.5772141,0.9640084,0.1510168],[0.9880533,0.8368791,0.8820018],[0.8607176,0.4635096,0.05747513],[0.2719098,0.5916616,0.8150284],[0.4459353,0.4131556,0.05966042],[0.1127767,0.002448122,0.2124518],[0.1898928,0.1660431,0.3602627],[0.2611665,0.4287802,0.3922152],[0.2517463,0.1570481,0.05633202],[0.1090992,0.1783884,0.4360036],[0.6355528,0.1116605,0.0698573],[0.008809316,0.9173065,0.3015817],[0.5332641,0.3693376,0.2087455],[0.9418411,0.5749845,0.909635],[0.9297088,0.2036425,0.6342672],[0.7937324,0.6349941,0.05023822],[0.5211098,0.7621584,0.6722742],[0.626762,0.1481719,0.139433],[0.2448719,0.4029988,0.8235071],[0.646358,0.5523618,0.1392542],[0.6887808,0.9741755,0.03411647],[0.848675,0.2138641,0.5287745],[0.6280065,0.7061405,0.5598994],[0.8532525,0.252234,0.5204226],[0.08847342,0.2737242,0.3841813],[0.3631095,0.5684832,0.7240314],[0.04832733,0.9772319,0.8912181],[0.7970614,0.2009498,0.5014495],[0.1899122,0.2285264,0.1639436],[0.7965345,0.188572,0.1836517],[0.9293551,0.1404706,0.1490238],[0.1318114,0.3068376,0.1083852],[0.5199742,0.3508246,0.6742347],[0.8508574,0.9175735,0.7092849],[0.2256492,0.1005429,0.04702054],[0.6812914,0.3073234,0.2815548],[0.4469795,0.609965,0.2889972],[0.2023303,0.1331885,0.5153117],[0.8312724,0.1196594,0.9719169],[0.9931236,0.6066545,0.3832877],[0.3104789,0.3440695,0.17443],[0.1074452,0.9031231,0.6939931],[0.7439377,0.2396454,0.3976088],[0.0279117,0.2219495,0.1536481],[0.8498195,0.4052949,0.7277521],[0.7973698,0.1789726,0.4774815],[0.7869422,0.4246944,0.8483545],[0.4688047,0.7431819,0.7421387],[0.582478,0.8688849,0.1669268],[0.3714654,0.5447046,0.1681659],[0.8928334,0.7836859,0.5958081],[0.005447067,0.3871703,0.4022754],[0.9728056,0.6824088,0.03188233],[0.7655979,0.6702726,0.5406112],[0.4986714,0.8169099,0.1614092],[0.2495534,0.6878175,0.5277274],[0.8643349,0.179031,0.1360152],[0.1524649,0.9447957,0.7614663],[0.877284,0.03631972,0.5483971],[0.225913,0.9063892,0.02861712],[0.1231631,0.1886397,0.5377533],[0.2426627,0.2580838,0.4619063],[0.07598623,0.1054296,0.3756129],[0.1260295,0.09798093,0.3925755],[0.9163371,0.6857722,0.8698512],[0.7639276,0.0175238,0.02267212],[0.8757943,0.6519427,0.0531094],[0.791319,0.2015895,0.3435059],[0.2150513,0.7069254,0.01822923],[0.2168552,0.8540304,0.1415181],[0.6369934,0.5504565,0.278185],[0.527912,0.6958793,0.2358698],[0.3649576,0.1300396,0.9276274],[0.05525132,0.2768639,0.4873848],[0.3897901,0.04756851,0.5482175],[0.4181431,0.2814613,0.6438522],[0.8385223,0.1376474,0.4273489],[0.5719252,0.5508786,0.5046999],[0.623111,0.05819936,0.456885],[0.7496884,0.2501413,0.4528331],[0.5399616,0.1563128,0.7111795],[0.1667123,0.1390772,0.4656253],[0.572724,0.08634555,0.7514718],[0.4245161,0.9564748,0.04470663],[0.7438336,0.400869,0.6357488],[0.6799436,0.7779377,0.5568159],[0.4986688,0.391234,0.8240591],[0.5665962,0.6644925,0.8277901],[0.9891843,0.02422757,0.07607652],[0.2478636,0.1096302,0.3092406],[0.5543849,0.7853202,0.8455557],[0.2243842,0.5438006,0.3588405],[0.9335379,0.2556463,0.1161516],[0.6674505,0.2834468,0.5836302],[0.009319689,0.7733302,0.9489557],[0.7916737,0.4490129,0.6510177],[0.268399,0.9584382,0.2235461],[0.8144639,0.8104439,0.5269571],[0.6101261,0.4381012,0.746731],[0.2032254,0.6890881,0.09712645],[0.2691386,0.8421859,0.1605125],[0.7893857,0.1417643,0.445704],[0.6781801,0.265845,0.5387418],[0.1038162,0.3408839,0.4974425],[0.664629,0.0920229,0.9443994],[0.3090551,0.04236587,0.3870697],[0.7083016,0.2741185,0.9216856],[0.3294093,0.4183654,0.3136677],[0.7104445,0.1157977,0.9679237],[0.8206233,0.7559721,0.03410616],[0.8201889,0.6455923,0.9404398],[0.4589804,0.933157,0.5429799],[0.255881,0.9785376,0.07881562],[0.8226316,0.6593776,0.4061187],[0.884883,0.68674,0.05447629],[0.5366461,0.2407877,0.6126193],[0.611062,0.5016919,0.3286982],[0.8415577,0.7718162,0.2397421],[0.17941,0.4686844,0.7793702],[0.6843942,0.06192318,0.9862406],[0.8435186,0.2535193,0.05269436],[0.7870569,0.02535332,0.5262576],[0.6997044,0.8687286,0.3190735],[0.6086001,0.9150085,0.3854055],[0.3999627,0.2442574,0.5599252],[0.3753399,0.4284283,0.3742581],[0.198584,0.7512906,0.1406495],[0.9157157,0.3641839,0.8143049],[0.0316989,0.8104318,0.9444556],[0.5083786,0.948253,0.3999255],[0.1833674,0.7938512,0.3463233],[0.3197395,0.1293094,0.3771501],[0.4707104,0.1359682,0.7553539],[0.1549076,0.9384621,0.2641518],[0.9870385,0.9589605,0.5924452],[0.05987407,0.1461096,0.1107785],[0.1143497,0.1156997,0.9811177],[0.3785148,0.2919713,0.3559611],[0.6745352,0.4611044,0.255742],[0.3872116,0.965178,0.1575038],[0.08803946,0.6762688,0.09020875],[0.694334,0.9503214,0.3060817],[0.1543014,0.5670496,0.6996848],[0.123203,0.3606174,0.2490901],[0.08384347,0.7520081,0.3150007],[0.1383442,0.3821934,0.9778967],[0.4853702,0.3948451,0.6003619],[0.5011882,0.2581383,0.1030055],[0.5925961,0.02698919,0.08947034],[0.9774102,0.4153953,0.5731856],[0.8646322,0.5695168,0.3245082],[0.2240613,0.3363567,0.5498168],[0.8997468,0.01293074,0.9946056],[0.9026006,0.8156065,0.1293845],[0.6032562,0.60717,0.2332968],[0.616497,0.05272305,0.3242261],[0.7713637,0.9214613,0.8151299],[0.4437077,0.3137707,0.6940348],[0.112856,0.6741397,0.1292429],[0.09028084,0.8570399,0.9715905],[0.6894628,0.1585298,0.5844234],[0.07077761,0.715845,0.5674541],[0.3859416,0.8388212,0.01025186],[0.8812929,0.886176,0.3269034],[0.2828522,0.146271,0.9053653],[0.8811206,0.4889778,0.1287211],[0.8184969,0.7288231,0.9817606],[0.027677,0.794619,0.9769028],[0.5088043,0.3949464,0.6567281],[0.2152073,0.5813779,0.34922],[0.479081,0.7922525,0.259438],[0.8514454,0.3076966,0.932013],[0.2247722,0.003574531,0.1740142],[0.8357224,0.7038785,0.4693035],[0.2850866,0.05084247,0.1320105],[0.6203282,0.6799995,0.4052313],[0.8417121,0.6740907,0.3908655],[0.3928452,0.7007608,0.9481952],[0.5062841,0.9820394,0.2822124],[0.6802782,0.2851242,0.04239904],[0.6229806,0.6002409,0.2189276],[0.4151322,0.7045116,0.3420456],[0.5426524,0.954741,0.2209489],[0.004468052,0.5139679,0.7027399],[0.5778334,0.3606974,0.7347071],[0.7107031,0.706886,0.5632248],[0.3672411,0.1446274,0.4467101],[0.8362501,0.5756233,0.2872394],[0.4289004,0.6360493,0.3256666],[0.1435122,0.2541205,0.09037109],[0.4805129,0.5699418,0.2637929],[0.6599458,0.3337511,0.2368524],[0.3598008,0.6797889,0.08356781],[0.5135612,0.7384623,0.7688617],[0.8921538,0.2885388,0.5072011],[0.9515328,0.109326,0.8990846],[0.2952311,0.3356393,0.009501632],[0.04178278,0.9592957,0.0239505],[0.05750178,0.13323,0.5098522],[0.2385067,0.02077828,0.3995838],[0.7200331,0.4379356,0.7610236],[0.06517167,0.3413539,0.405179],[0.5253382,0.253823,0.04662498],[0.1345057,0.956517,0.8202046],[0.06359477,0.8765337,0.2259262],[0.642313,0.891848,0.04286834],[0.8368759,0.360198,0.3598628],[0.352295,0.6917877,0.5472175],[0.231095,0.7919766,0.9036624],[0.2788297,0.8575465,0.5999774],[0.778231,0.4740622,0.0890776],[0.9086965,0.5266357,0.4221227],[0.2345455,0.8404368,0.9811774],[0.5407688,0.07880231,0.3007811],[0.6906641,0.9877006,0.6104504],[0.02964015,0.5655652,0.0410066],[0.7185504,0.004326933,0.4153787],[0.8940973,0.4204474,0.7928818],[0.3788327,0.85595,0.4076351],[0.9707994,0.0264636,0.7713273],[0.2759047,0.3128908,0.07674231],[0.4777076,0.2379093,0.00425959],[0.8107567,0.5843107,0.2172831],[0.7526672,0.104906,0.1259282],[0.5821646,0.1091026,0.0558827],[0.9745858,0.4210176,0.9896051],[0.0615777,0.3141613,0.452683],[0.5917662,0.4066314,0.4131185],[0.3087108,0.2949614,0.4038092],[0.2624937,0.8892192,0.5878615],[0.7664469,0.434385,0.6421778],[0.7729462,0.0911948,0.5011417],[0.8718159,0.3378896,0.2723646],[0.1232033,0.8079789,0.435391],[0.3167284,0.2059089,0.2353772],[0.2790607,0.3885424,0.5334129],[0.1759923,0.5585449,0.617483],[0.882231,0.3817382,0.3172487],[0.04501222,0.1778622,0.1078495],[0.725508,0.5737641,0.6266844],[0.8856388,0.3125403,0.2408604],[0.1675298,0.7229896,0.09564218],[0.2532063,0.2092023,0.351648],[0.3461677,0.3725057,0.373862],[0.8393751,0.8371015,0.5536796],[0.701614,0.9455406,0.6538313],[0.2219456,0.8082437,0.5489432],[0.7579771,0.2859723,0.05701023],[0.5230885,0.2951223,0.169255],[0.02707036,0.3342298,0.1747278],[0.5568224,0.5470837,0.05717333],[0.4209177,0.3591011,0.8792979],[0.7025234,0.1061665,0.5291857],[0.07505986,0.3472486,0.3606921],[0.6930292,0.2914793,0.3678991],[0.9500815,0.432723,0.3690906],[0.9003539,0.08086746,0.6286795],[0.6624048,0.3733809,0.1617432],[0.05530744,0.4360713,0.1489772],[0.04214885,0.01389125,0.4928742],[0.6240939,0.02134971,0.1679796],[0.1893312,0.1569691,0.4246679],[0.9524087,0.5174455,0.1697397],[0.6907473,0.7563787,0.5915025],[0.1219934,0.5714145,0.2787178],[0.4211693,0.8649276,0.6771061],[0.2389591,0.7028099,0.3799395],[0.9075295,0.8982301,0.1479805],[0.3368321,0.8242054,0.5950226],[0.7972099,0.9399766,0.3524249],[0.5967492,0.2357857,0.7605378],[0.03000334,0.04540993,0.2442439],[0.8722068,0.5711665,0.7396023],[0.9756932,0.3628621,0.2341644],[0.7067322,0.386458,0.041742],[0.7722691,0.4460253,0.1486393],[0.8342686,0.194949,0.3475249],[0.06885353,0.8583329,0.04042793],[0.5531633,0.4956573,0.9828901],[0.9807071,0.7829766,0.1314494],[0.1751627,0.2008764,0.1939813],[0.2895544,0.3162114,0.1179191],[0.4146753,0.119982,0.8552463],[0.9662099,0.6908314,0.7164552],[0.8907881,0.2099846,0.01057623],[0.04984891,0.2633872,0.3724208],[0.4795951,0.2403383,0.7062563],[0.3811385,0.3841362,0.149194],[0.111769,0.5501095,0.8697262],[0.912301,0.769827,0.860171],[0.5610743,0.04269535,0.3728599],[0.8975698,0.1836712,0.3581578],[0.117804,0.263405,0.4836182],[0.04164989,0.8549811,0.5510612],[0.1842253,0.7771528,0.1633653],[0.4661035,0.4001307,0.1991804],[0.2363108,0.2591196,0.4711879],[0.4326396,0.2343428,0.6965451],[0.06664436,0.5174368,0.7802794],[0.4644529,0.930394,0.01971098],[0.9792625,0.5912515,0.3968413],[0.6982156,0.3180582,0.9363747],[0.9385859,0.9019738,0.3346283],[0.6715633,0.7468174,0.9785843],[0.7041028,0.70008,0.2009973],[0.7416335,0.726841,0.4479916],[0.002022203,0.1302251,0.9399925],[0.1612432,0.5808803,0.8541881],[0.6404414,0.7396501,0.4785343],[0.999822,0.9062732,0.9871581],[0.9062259,0.2510424,0.8695824],[0.8678694,0.1894719,0.5635728],[0.8610322,0.4158821,0.1527234],[0.8036636,0.5835308,0.8577595],[0.7783054,0.9699059,0.2338534],[0.6268761,0.8689761,0.1519084],[0.5498987,0.3327376,0.7330548],[0.08958354,0.6465266,0.9241467],[0.03200383,0.02838162,0.3502955],[0.9080837,0.1185535,0.1108892],[0.6012642,0.8866001,0.3656538],[0.9950901,0.6949722,0.6088338],[0.3498011,0.003132959,0.1463962],[0.4872715,0.3005398,0.1940881],[0.3353269,0.5341323,0.6461917],[0.7527327,0.5855935,0.8958412],[0.1404653,0.001620509,0.9785998],[0.1098449,0.771822,0.4669435],[0.4803346,0.8654415,0.2939362],[0.1258991,0.327092,0.368734],[0.965899,0.5626671,0.5885755],[0.9339,0.1560485,0.8919647],[0.4055457,0.6599495,0.1437558],[0.2854518,0.67629,0.1360819],[0.1468085,0.5452735,0.02897464],[0.3803596,0.509112,0.3691459],[0.7787759,0.09336269,0.5157034],[0.2315899,0.7395467,0.1097114],[0.8268521,0.4212603,0.0003576479],[0.9011801,0.1522511,0.8484592],[0.9805238,0.4711404,0.5926341],[0.6575539,0.2636992,0.1319096],[0.9132458,0.5339597,0.2286678],[0.2348486,0.4041704,0.3174894],[0.6831527,0.9886117,0.6638672],[0.1519558,0.5775766,0.8522779],[0.573355,0.7585201,0.2508304],[0.8083931,0.5833808,0.197785],[0.2011301,0.06672417,0.3792828],[0.5630933,0.0509206,0.6412811],[0.1093802,0.6612357,0.5692042],[0.9408494,0.5510031,0.2057537],[0.8581802,0.09596458,0.5532867],[0.6887821,0.7320033,0.2826708],[0.4722558,0.9219806,0.04105844],[0.4533991,0.4511177,0.9420172],[0.5472632,0.9954231,0.08556546],[0.9712889,0.3020952,0.003584702],[0.07816486,0.7307197,0.9347373],[0.3703812,0.6651318,0.3351983],[0.8054785,0.6103267,0.5350451],[0.2200541,0.7059799,0.133855],[0.8818889,0.9740203,0.767288],[0.8859101,0.5076312,0.7979087],[0.02289913,0.5813037,0.7037878],[0.5912546,0.6328166,0.3627546],[0.7123618,0.9450372,0.3742678],[0.6317844,0.8254454,0.3809356],[0.3141735,0.8063547,0.1043045],[0.008777333,0.003269027,0.5460063],[0.777505,0.09686333,0.5802317],[0.02214603,0.4865232,0.8008839],[0.7457403,0.0931792,0.9556206],[0.3348725,0.1173491,0.1438058],[0.8362657,0.2287827,0.9861339],[0.9354731,0.6706001,0.6756568],[0.5994512,0.3760856,0.8329521],[0.002998041,0.9914191,0.2714364],[0.8129847,0.5523942,0.09996776],[0.4874673,0.07329261,0.2637025],[0.1898265,0.861712,0.158208],[0.1712121,0.6230633,0.4441638],[0.6990395,0.6492897,0.2239211],[0.04484119,0.5214429,0.8816596],[0.3284501,0.4892816,0.8928865],[0.2003263,0.9238617,0.3896406],[0.9464346,0.8365219,0.8709311],[0.7375295,0.4704418,0.07604808],[0.01586564,0.5000671,0.05018213],[0.07870673,0.304803,0.1008227],[0.4282252,0.07355248,0.02314295],[0.8554245,0.5998793,0.7320669],[0.6097917,0.6110688,0.9800761],[0.6385395,0.5189751,0.8112603],[0.9755207,0.9094111,0.1019356],[0.6978943,0.001212666,0.04578892],[0.7642244,0.9940141,0.06573977],[0.1352308,0.1226549,0.516371],[0.4952544,0.05013798,0.1398177],[0.7007978,0.804586,0.1571259],[0.570461,0.9054978,0.02374578],[0.5388666,0.2998436,0.6164582],[0.5075229,0.02585634,0.7500738],[0.2748364,0.5226046,0.2267591],[0.6479139,0.3964883,0.5104172],[0.1093687,0.4789625,0.263835],[0.6064209,0.9953534,0.6703193],[0.6407182,0.72223,0.2107718],[0.1696519,0.7764432,0.1996092],[0.4103282,0.9034811,0.4853603],[0.4523315,0.7923985,0.3688036],[0.4571875,0.1600944,0.1294079],[0.9440072,0.7277213,0.2534567],[0.4843549,0.9145653,0.9989919],[0.9871989,0.3132344,0.2738053],[0.18587,0.7123095,0.1477248],[0.3603475,0.219793,0.09760268],[0.6468731,0.02346477,0.9097803],[0.6152851,0.3352257,0.3801033],[0.7075157,0.5122435,0.8389233],[0.3296887,0.8466026,0.2182844],[0.01622046,0.3989755,0.447916],[0.4141531,0.9156489,0.1470558],[0.1891999,0.279223,0.9558859],[0.4385836,0.2450408,0.3436106],[0.5085086,0.3882283,0.002141457],[0.7132118,0.3095436,0.2606763],[0.1025925,0.46432,0.1030474],[0.6554641,0.9654243,0.3732085],[0.1229307,0.07058309,0.2559245],[0.3718702,0.4446562,0.1254205],[0.8693256,0.1085515,0.9069666],[0.8004019,0.9556031,0.7198588],[0.6082353,0.720969,0.3135169],[0.963805,0.2803282,0.674181],[0.1955235,0.8735915,0.4319008],[0.7203729,0.2643699,0.1021005],[0.4107635,0.03008889,0.3430972],[0.5221941,0.9567546,0.2919371],[0.8189524,0.03638248,0.2594411],[0.02299816,0.07369407,0.07907515],[0.2683749,0.9121143,0.8377782],[0.5573077,0.8716689,0.6977855],[0.09890632,0.7957814,0.09542608],[0.7417747,0.2041998,0.02397048],[0.6134654,0.8849981,0.3410067],[0.6745008,0.6485907,0.4434016],[0.7633588,0.8364934,0.4227943],[0.8789722,0.0813617,0.3712786],[0.2988829,0.7809265,0.1782501],[0.9798177,0.6977059,0.845987],[0.3978385,0.7601932,0.503835],[0.3140472,0.1506542,0.5273804],[0.7382069,0.8002579,0.9920936],[0.9915762,0.3440266,0.05251123],[0.7971563,0.6627979,0.9597374],[0.2456043,0.4078822,0.8286775],[0.5143243,0.6750889,0.3680185],[0.8418086,0.8047842,0.2013299],[0.4166574,0.887156,0.7137542],[0.4074057,0.5906084,0.2218859],[0.05991314,0.5118791,0.8484272],[0.7602676,0.2355108,0.2161297],[0.4569128,0.3954383,0.1203131],[0.6230573,0.1919032,0.9503594],[0.4022436,0.4494502,0.1431572],[0.6989695,0.7132365,0.504388],[0.9924118,0.9828471,0.03988377],[0.6635498,0.4363963,0.6033368],[0.8426574,0.09320974,0.9949651],[0.4847158,0.005049093,0.6591319],[0.7422459,0.8505942,0.02040913],[0.5679761,0.9305258,0.4349755],[0.01658792,0.04501205,0.09195523],[0.3914092,0.4285314,0.738937],[0.7795367,0.2839584,0.3707983],[0.5652453,0.08891171,0.1728705],[0.3947785,0.8366242,0.1825859],[0.7945364,0.5992881,0.881057],[0.05081595,0.596207,0.129341],[0.04327673,0.04491724,0.07552645],[0.9524975,0.7901428,0.1199638],[0.1431417,0.01537926,0.9001122],[0.3374082,0.9591017,0.6834795],[0.8764121,0.9050407,0.3116206],[0.9044642,0.3288503,0.7552186],[0.7554722,0.9211066,0.8131778],[0.2868456,0.4053591,0.08700886],[0.1400089,0.4016947,0.8089546],[0.8582714,0.3490339,0.1250552],[0.2378043,0.7342828,0.4600169],[0.1303854,0.832009,0.2295302],[0.1825986,0.001949112,0.4261869],[0.7265909,0.4679423,0.1073152],[0.2391569,0.3549084,0.4335623],[0.9078784,0.6663331,0.6927013],[0.8612657,0.6922292,0.9496523],[0.171904,0.5349746,0.6941084],[0.1649713,0.5658841,0.01678043],[0.6932481,0.1694014,0.1973547],[0.5926282,0.5339214,0.9945816],[0.09486501,0.6528681,0.3718326],[0.8738722,0.614916,0.5892155],[0.8643004,0.3571738,0.3129403],[0.7438174,0.9019154,0.1663421],[0.1448396,0.62573,0.3325768],[0.1204262,0.4577306,0.8728039],[0.9318317,0.7358863,0.3259563],[0.08359949,0.554901,0.1966319],[0.1804264,0.6768677,0.05344877],[0.5304279,0.1970111,0.7497321],[0.4710418,0.5868275,0.9896713],[0.4309578,0.4248174,0.05460518],[0.7968376,0.4358988,0.1529393],[0.7285018,0.3026343,0.5080059],[0.5947081,0.2404894,0.3101921],[0.6666261,0.3940044,0.006250999],[0.1504667,0.6594449,0.965236],[0.3908215,0.8778233,0.07324158],[0.8748699,0.03757825,0.0146193],[0.5057289,0.8685477,0.1248035],[0.9812524,0.7533667,0.8296431],[0.4462835,0.3950073,0.8750625],[0.01962186,0.3738714,0.4872614],[0.0766587,0.6520039,0.03965764],[0.3151932,0.7580474,0.1113485],[0.2467767,0.8792401,0.4678466],[0.6606621,0.8600206,0.009000621],[0.2023306,0.4665552,0.6450259],[0.8981475,0.0921994,0.52089],[0.2635963,0.4124482,0.1107278],[0.979224,0.6802778,0.1235441],[0.7876536,0.1323127,0.1553956],[0.8347445,0.6217793,0.2941952],[0.09720937,0.1213271,0.9164401],[0.7073036,0.9898723,0.2077476],[0.6836618,0.8800549,0.1410724],[0.7565687,0.09807366,0.463263],[0.134476,0.9315296,0.3552936],[0.1841529,0.2810341,0.5339861],[0.3388878,0.6033257,0.5119682],[0.5689567,0.444641,0.4024922],[0.9514536,0.9153034,0.02077597],[0.05764429,0.7737213,0.9503134],[0.9470703,0.2428845,0.05095523],[0.5409303,0.3650789,0.3346318],[0.4896387,0.8362569,0.6129729],[0.8617996,0.2687113,0.5497404],[0.1354447,0.1963761,0.3715279],[0.3757158,0.5003306,0.2122137],[0.8133289,0.720393,0.4559765],[0.3746369,0.3463143,0.3368754],[0.2781214,0.7662342,0.4778734],[0.5419187,0.8152454,0.1859433],[0.9848443,0.9865587,0.4246545],[0.9831887,0.6903068,0.7106765],[0.6630965,0.5746457,0.10702],[0.1007329,0.3426162,0.4718232],[0.4885853,0.7765095,0.07353193],[0.646936,0.7505845,0.2397815],[0.1787546,0.8185402,0.2911724],[0.6227978,0.8921314,0.1839604],[0.01757807,0.1994732,0.2629862],[0.7983946,0.2806315,0.5997507],[0.3948658,0.2212407,0.3868829],[0.3003035,0.6980917,0.5264721],[0.1649283,0.8016341,0.2482744],[0.8608962,0.7351644,0.5431377],[0.5948412,0.6048048,0.6536132],[0.02427213,0.7231531,0.06860469],[0.2616418,0.2629994,0.2793049],[0.8095182,0.5187546,0.8006856],[0.1055581,0.6866038,0.06717064],[0.9736134,0.1781756,0.2119161],[0.9587163,0.9114941,0.3695781],[0.2770989,0.3896606,0.1611973],[0.6998586,0.4369403,0.858333],[0.7878158,0.5235511,0.8045433],[0.9373181,0.329714,0.7221518],[0.04410455,0.9518002,0.6222757],[0.7577251,0.7987091,0.03096572],[0.68283,0.6665297,0.3972919],[0.8648934,0.6985518,0.4023553],[0.9406583,0.1709769,0.708323],[0.3225898,0.9710565,0.8025261],[0.07730129,0.899613,0.2910788],[0.8194441,0.2173542,0.1348961],[0.1594302,0.8736352,0.07612028],[0.968281,0.3239083,0.819782],[0.8425965,0.2700415,0.1762823],[0.3427286,0.919786,0.6100376],[0.543712,0.4287878,0.2886489],[0.3881812,0.5434511,0.2811292],[0.06098921,0.2613012,0.1068534],[0.3129643,0.05075523,0.5904489],[0.2095001,0.9457818,0.08816009],[0.5057392,0.3639237,0.1217383],[0.5060917,0.9433605,0.5588754],[0.3515147,0.9497387,0.01079994],[0.09952402,0.2317563,0.5218825],[0.09660386,0.5726692,0.8116875],[0.1046332,0.1568016,0.9802425],[0.6502258,0.1320816,0.3140722],[0.4417999,0.223363,0.3220521],[0.4050542,0.7049772,0.1310506],[0.3830844,0.5693383,0.2807615],[0.6404404,0.3056727,0.5635602],[0.8828412,0.8139481,0.4066896],[0.9154798,0.2631308,0.6300376],[0.9086897,0.4638479,0.03578534],[0.891431,0.2135609,0.2860225],[0.5457854,0.4864144,0.193337],[0.2935387,0.3401692,0.0231246],[0.8014858,0.157885,0.1182548],[0.6968247,0.2262529,0.9901936],[0.9049159,0.8524192,0.8866068],[0.7861432,0.7156464,0.9634863],[0.8337231,0.3168902,0.4364631],[0.3479899,0.2541869,0.3150941],[0.8265161,0.7304065,0.3738916],[0.4277714,0.1100659,0.05255983],[0.3791581,0.5816537,0.1383352],[0.7018435,0.2629349,0.140754],[0.222402,0.1892026,0.995185],[0.2100355,0.2135195,0.549095],[0.5471898,0.046568,0.8336387],[0.4574795,0.2949133,0.7024872],[0.7661688,0.6295558,0.4799606],[0.9400573,0.2273693,0.135815],[0.143177,0.9681278,0.6209091],[0.633462,0.895883,0.9516718],[0.8715613,0.06579074,0.3514523],[0.6019812,0.8159906,0.5045606],[0.2419881,0.4188913,0.3992342],[0.3848269,0.5627536,0.3083453],[0.6222898,0.7667878,0.2297547],[0.3713199,0.5896947,0.0219754],[0.3661808,0.3394423,0.513129],[0.2041376,0.9087263,0.7101779],[0.9473315,0.6478429,0.7009748],[0.5777516,0.7774305,0.05453363],[0.06254588,0.09253263,0.1699014],[0.350758,0.961531,0.8101431],[0.7124114,0.6325803,0.2401294],[0.3507365,0.3906885,0.01287977],[0.7921059,0.4493259,0.750712],[0.1058607,0.6252477,0.02632205],[0.6196839,0.7148029,0.2986535],[0.7651018,0.1194663,0.4965323],[0.4764369,0.771054,0.8827561],[0.3372615,0.725863,0.97885],[0.7811131,0.03463888,0.06058018],[0.7622159,0.3234169,0.474398],[0.8897331,0.2131212,0.3201732],[0.368294,0.6084016,0.5890709],[0.5533566,0.09070392,0.1685765],[0.1892062,0.05120347,0.5386987],[0.7452682,0.9078513,0.2844285],[0.1292213,0.07813044,0.3342875],[0.1885016,0.2731214,0.09862859],[0.1954716,0.0977504,0.5624629],[0.7476602,0.8560746,0.5714936],[0.606062,0.2516818,0.0007076142],[0.4290569,0.4163084,0.4299097],[0.877269,0.6651017,0.4326306],[0.6130383,0.09655346,0.5300621],[0.1010395,0.2559853,0.3572788],[0.3859218,0.6092653,0.9853331],[0.5963485,0.7678604,0.1495827],[0.1452219,0.7011864,0.9887306],[0.1399227,0.1095112,0.4218689],[0.2954761,0.1863925,0.09948967],[0.3904811,0.9414756,0.6706014],[0.4921306,0.7043031,0.6322739],[0.7764632,0.05353354,0.9454409],[0.5674757,0.9458677,0.5936499],[0.6589653,0.5428843,0.7197235],[0.142778,0.6249906,0.4310954],[0.6478862,0.7520018,0.5550422],[0.5753781,0.6428936,0.1149073],[0.9110243,0.2922144,0.1658856],[0.6148176,0.7113149,0.4056941],[0.2916553,0.6025245,0.9742132],[0.6379832,0.7964326,0.01347817],[0.5349647,0.06005703,0.3726864],[0.5710453,0.6777078,0.2921865],[0.714864,0.5560918,0.7496186],[0.4281344,0.8229896,0.2544633],[0.3683299,0.4151402,0.7521948],[0.59443,0.4310326,0.01528303],[0.3830533,0.5829638,0.2710248],[0.1197591,0.1149003,0.2581219],[0.6924039,0.5545745,0.8257744],[0.3303028,0.8694375,0.3012603],[0.7436187,0.6822206,0.5655456],[0.7100012,0.5763988,0.8931471],[0.9529852,0.192776,0.1981504],[0.1883372,0.3837759,0.5574169],[0.7120694,0.03149105,0.4303229],[0.3630335,0.9936557,0.7765989],[0.05634804,0.4488167,0.7905964],[0.8402082,0.883473,0.208261],[0.8793039,0.8442072,0.4626269],[0.212134,0.7337624,0.2975561],[0.7632441,0.8155888,0.9967182],[0.5527397,0.6366978,0.7384346],[0.7328045,0.3194709,0.2271044],[0.8550742,0.6148975,0.4074123],[0.6056051,0.6388928,0.9515616],[0.2335246,0.8718236,0.1529955],[0.2421332,0.5774788,0.3319368],[0.04664214,0.1018921,0.9548021],[0.9445244,0.6202074,0.1430607],[0.2064646,0.4069621,0.8517151],[0.7282557,0.8794106,0.3471794],[0.4043675,0.7485863,0.05210817],[0.09487712,0.8624592,0.3294469],[0.07201727,0.9387043,0.07072176],[0.1234582,0.142942,0.03121849],[0.7367825,0.3146619,0.2866315],[0.8691689,0.1306546,0.9173884],[0.6594132,0.6006732,0.435275],[0.4425798,0.9401539,0.2808141],[0.8954968,0.7782944,0.4100751],[0.6253563,0.2389108,0.1988841],[0.9200588,0.2163982,0.1032619],[0.05658745,0.1282517,0.0823506],[0.7631497,0.5215948,0.6817681],[0.2118441,0.4423235,0.7597719],[0.6753439,0.8095644,0.5820255],[0.7932175,0.8637129,0.2894087],[0.2741465,0.3086627,0.5826185],[0.8348137,0.9070828,0.2334401],[0.1672576,0.7080984,0.4861816],[0.4996527,0.9657624,0.9993041],[0.3453745,0.9388385,0.8343057],[0.3616396,0.8562248,0.1291977],[0.01312375,0.04156399,0.5768628],[0.7018677,0.3147714,0.257033],[0.5225391,0.06393015,0.1039596],[0.1440795,0.3117091,0.05925139],[0.727734,0.2610545,0.5482287],[0.9326493,0.4941159,0.06145179],[0.9530427,0.5539443,0.9846425],[0.8579511,0.7527741,0.1213191],[0.304114,0.9759378,0.1116001],[0.4467027,0.2698409,0.1688536],[0.3047281,0.9570543,0.2300928],[0.4243942,0.7238415,0.3399117],[0.715962,0.3419712,0.2382799],[0.8185193,0.7477506,0.3903706],[0.2318523,0.2566975,0.3975715],[0.4694611,0.3346855,0.1420296],[0.1549694,0.1074988,0.4683245],[0.07745758,0.003525609,0.01572911],[0.9723916,0.2905175,0.2064929],[0.3057032,0.6285105,0.3637211],[0.4771083,0.1481287,0.7498872],[0.2733387,0.4809984,0.02659419],[0.2396526,0.9090971,0.2394431],[0.002745515,0.7428486,0.4487954],[0.4635013,0.6043754,0.06064134],[0.1450482,0.3252193,0.360779],[0.6819564,0.8977618,0.02050897],[0.5720799,0.6895346,0.1727647],[0.2647404,0.8236326,0.9596339],[0.8026654,0.4450908,0.8794767],[0.9256909,0.6965672,0.06793532],[0.2653168,0.7452053,0.2557293],[0.2370935,0.6317569,0.09692019],[0.3058413,0.7933394,0.4472271],[0.01342801,0.5187605,0.2374093],[0.1507909,0.2400866,0.5780475],[0.8923087,0.2444188,0.4090492],[0.6984617,0.2930485,0.07015125],[0.1990142,0.1360231,0.07929788],[0.2753566,0.08799621,0.04591766],[0.1835264,0.1612839,0.404529],[0.2508898,0.9802253,0.837227],[0.9766802,0.9063262,0.4804054],[0.3499265,0.06410669,0.5729151],[0.02055014,0.8283018,0.3080007],[0.312508,0.1445577,0.23998],[0.9202878,0.9284864,0.4560237],[0.01772781,0.4361217,0.08445555],[0.4646213,0.259203,0.2768611],[0.4431858,0.3894852,0.6807607],[0.39501,0.113275,0.3748507],[0.9372081,0.8286007,0.8807833],[0.01482022,0.6912701,0.5390469],[0.8627195,0.6437809,0.4999591],[0.9078777,0.5416033,0.9833603],[0.02028302,0.4892141,0.0255557],[0.6862425,0.7078261,0.2415482],[0.4444191,0.9390196,0.08189515],[0.2891088,0.6915336,0.4042208],[0.2509878,0.4044805,0.6651818],[0.6859226,0.8490096,0.3185745],[0.8523493,0.6903115,0.1262356],[0.07840697,0.06507296,0.960499],[0.4348946,0.2656779,0.03740625],[0.879352,0.3211545,0.3754799],[0.0224725,0.7470324,0.2377684],[0.8136806,0.07210518,0.4233399],[0.5542477,0.1428635,0.6864997],[0.6455499,0.8799158,0.2746847],[0.3047399,0.9089209,0.6842065],[0.04782989,0.2550043,0.1804728],[0.6274424,0.6744105,0.1943641],[0.547159,0.8432369,0.7425972],[0.1113352,0.1668285,0.4849556],[0.7655746,0.182412,0.4607148],[0.6977473,0.5206379,0.8068109],[0.3765805,0.2785719,0.5274165],[0.9234649,0.965398,0.9192127],[0.3403862,0.9826154,0.3325526],[0.5894636,0.3425842,0.02565965],[0.1932934,0.9918802,0.1722992],[0.08454818,0.02391287,0.1342129],[0.7320578,0.7197176,0.1076004],[0.894274,0.9136862,0.1133154],[0.06982326,0.8877488,0.9335943],[0.6617953,0.1059647,0.3433868],[0.04798799,0.8077517,0.158458],[0.07690156,0.7743883,0.9192176],[0.184206,0.1763955,0.9116448],[0.2876672,0.3541079,0.2188042],[0.9770598,0.2077172,0.1195427],[0.1679376,0.8824788,0.2761688],[0.9355891,0.5324804,0.3194948],[0.2299964,0.5946591,0.2297624],[0.7806079,0.01662784,0.4078861],[0.8853427,0.7296007,0.3253926],[0.9878823,0.06979974,0.6905417],[0.7418671,0.2040863,0.2273316],[0.1746745,0.8068922,0.9440175],[0.9038535,0.4522993,0.02177001],[0.2094224,0.8147113,0.4501432],[0.1259422,0.2510449,0.9124339],[0.4772216,0.2052998,0.6149561],[0.09684517,0.5564294,0.7198447],[0.973294,0.8003299,0.2328485],[0.4136997,0.3840894,0.7377988],[0.4926799,0.3556009,0.2567271],[0.07022183,0.8468416,0.4104985],[0.4661995,0.1812757,0.6998419],[0.9837285,0.3817531,0.7633581],[0.6196785,0.2102056,0.1246073],[0.7294798,0.09575338,0.0469557],[0.0634466,0.1647829,0.1671746],[0.5022808,0.9569576,0.3571503],[0.07496174,0.4601299,0.8478738],[0.3238432,0.09073412,0.1632024],[0.2058291,0.01145239,0.1735193],[0.3661427,0.5123239,0.8526506],[0.8407127,0.3083669,0.1038387],[0.6291417,0.06078131,0.3163382],[0.7119459,0.1752802,0.2122921],[0.2219913,0.8224792,0.2973896],[0.735857,0.3688692,0.04336479],[0.01741841,0.6610817,0.4831148],[0.367685,0.9177188,0.2213023],[0.7937216,0.5334988,0.2780769],[0.3824705,0.08803483,0.3719232],[0.6571894,0.07695857,0.2575436],[0.06907396,0.02305604,0.09112689],[0.1836423,0.03042321,0.5665804],[0.04558595,0.9324376,0.3008339],[0.07300608,0.8410136,0.07939438],[0.9402593,0.9560778,0.1548008],[0.499194,0.6678943,0.8584014],[0.8932449,0.4067459,0.3098028],[0.2168622,0.7813427,0.194105],[0.9845688,0.4310915,0.9534854],[0.6600198,0.7088512,0.3288195],[0.834246,0.9276955,0.1965624],[0.3305761,0.1594952,0.2271687],[0.4747568,0.8255891,0.8298818],[0.6851702,0.5940815,0.6416696],[0.2448724,0.131447,0.5076517],[0.9985381,0.2582079,0.3549165],[0.7565879,0.1383926,0.3378993],[0.8582726,0.6576093,0.4098599],[0.03078299,0.4215254,0.308689],[0.6328759,0.80744,0.1342005],[0.5524927,0.2988904,0.8847572],[0.1584617,0.8980558,0.3988017],[0.406028,0.6250166,0.8279935],[0.5595255,0.5110667,0.09634256],[0.7670653,0.8998862,0.4572381],[0.6302104,0.8922837,0.2480944],[0.7446564,0.4246858,0.3901175],[0.8499277,0.5507814,0.2177557],[0.7274488,0.4236023,0.3657607],[0.2739173,0.1679625,0.4365861],[0.3893525,0.3379947,0.9002159],[0.7324348,0.5782148,0.3077096],[0.3819163,0.7118912,0.9606726],[0.08534239,0.07271635,0.2729814],[0.3601942,0.8287733,0.2037097],[0.9983507,0.1241636,0.170898],[0.9779375,0.5441275,0.3697605],[0.2645564,0.1380209,0.1238847],[0.06583135,0.259555,0.1527514],[0.7703986,0.8449654,0.4078432],[0.672639,0.6023911,0.5838399],[0.2707793,0.9051656,0.2714971],[0.8429348,0.6448606,0.2281958],[0.9063564,0.4370343,0.5786622],[0.6166458,0.8868197,0.3941298],[0.934682,0.9796647,0.5568718],[0.1469417,0.08881273,0.3979052],[0.463529,0.924481,0.0246367],[0.3187331,0.789187,0.5363152],[0.4868439,0.371725,0.0961632],[0.9080275,0.870792,0.810964],[0.6244642,0.580241,0.04364656],[0.3037448,0.6671388,0.9676614],[0.1186313,0.9473036,0.7078985],[0.872466,0.7203887,0.5479698],[0.3503369,0.6840641,0.5190206],[0.02756294,0.9918048,0.357626],[0.3501461,0.9921781,0.1213199],[0.3773305,0.3788023,0.1761495],[0.726885,0.9149681,0.128943],[0.1770431,0.857592,0.09952889],[0.9029484,0.9184938,0.5346761],[0.02755318,0.3940175,0.1452875],[0.6211736,0.8925311,0.38829],[0.2559045,0.09446543,0.9244592],[0.5280436,0.07620411,0.05985307],[0.2278901,0.04276507,0.4910267]],"ignoreExtent":false,"flags":0},"9":{"id":9,"type":"text","material":{"lit":false},"vertices":[[0.5009221,-0.167306,-0.1689638]],"colors":[[0,0,0,1]],"texts":[["var_0015"]],"cex":[[1]],"adj":[[0.5,0.5]],"centers":[[0.5009221,-0.167306,-0.1689638]],"family":[["sans"]],"font":[[1]],"ignoreExtent":true,"flags":40},"10":{"id":10,"type":"text","material":{"lit":false},"vertices":[[-0.1671049,0.4983179,-0.1689638]],"colors":[[0,0,0,1]],"texts":[["var_0016"]],"cex":[[1]],"adj":[[0.5,0.5]],"centers":[[-0.1671049,0.4983179,-0.1689638]],"family":[["sans"]],"font":[[1]],"ignoreExtent":true,"flags":40},"11":{"id":11,"type":"text","material":{"lit":false},"vertices":[[-0.1671049,-0.167306,0.4998308]],"colors":[[0,0,0,1]],"texts":[["var_0017"]],"cex":[[1]],"adj":[[0.5,0.5]],"centers":[[-0.1671049,-0.167306,0.4998308]],"family":[["sans"]],"font":[[1]],"ignoreExtent":true,"flags":40},"12":{"id":12,"type":"points","material":{"lit":false,"size":6},"vertices":[[0.5213653,0.8221655,0.8365842],[0.3987098,0.1855501,0.4858116],[0.9202044,0.2520178,0.2438436],[0.1720597,0.9787687,0.3991734],[0.9954553,0.1643996,0.1310551],[0.7078151,0.3920895,0.03815126],[0.3937303,0.2111141,0.2569995],[0.8509175,0.9536532,0.6905547],[0.136928,0.4739677,0.8277075],[0.6114396,0.7018894,0.2258187],[0.7485089,0.9178217,0.1600771],[0.03262887,0.2120046,0.388447],[0.1722189,0.4067134,0.08863472],[0.2332716,0.1848344,0.8804049],[0.2832615,0.9460198,0.8276815],[0.5082195,0.1894642,0.7258652],[0.4770071,0.1496573,0.6294762],[0.7574398,0.3947192,0.1924464],[0.1498318,0.2409873,0.8137141],[0.6432346,0.2676371,0.9583846],[0.8517047,0.6855458,0.5413228],[0.4412634,0.751499,0.7884656],[0.5939756,0.9972243,0.3435504],[0.1283589,0.7168143,0.4213458],[0.308209,0.06448657,0.9809136],[0.361118,0.6180246,0.5825453],[0.2410122,0.4567871,0.8035313],[0.92677,0.7004799,0.6307325],[0.159856,0.8010102,0.06564418],[0.7632383,0.6643394,0.5151525],[0.2555983,0.5712122,0.6697811],[0.3976715,0.1158252,0.05177924],[0.5740297,0.7586238,0.1467678],[0.5367217,0.7372069,0.04172584],[0.7212515,0.2783328,0.7478626],[0.1802856,0.245905,0.7077527],[0.6376799,0.885767,0.3647935],[0.7762479,0.9089336,0.6902051],[0.4503707,0.9911413,0.3136873],[0.2393965,0.812039,0.4650568],[0.1756934,0.06197349,0.008356333],[0.9626651,0.6203654,0.6132363],[0.8257303,0.2194321,0.2688665],[0.4551463,0.2222483,0.1631426],[0.26008,0.4686306,0.8440437],[0.6408656,0.2775612,0.1929819]],"colors":[[1,0,0,1]],"centers":[[0.5213653,0.8221655,0.8365842],[0.3987098,0.1855501,0.4858116],[0.9202044,0.2520178,0.2438436],[0.1720597,0.9787687,0.3991734],[0.9954553,0.1643996,0.1310551],[0.7078151,0.3920895,0.03815126],[0.3937303,0.2111141,0.2569995],[0.8509175,0.9536532,0.6905547],[0.136928,0.4739677,0.8277075],[0.6114396,0.7018894,0.2258187],[0.7485089,0.9178217,0.1600771],[0.03262887,0.2120046,0.388447],[0.1722189,0.4067134,0.08863472],[0.2332716,0.1848344,0.8804049],[0.2832615,0.9460198,0.8276815],[0.5082195,0.1894642,0.7258652],[0.4770071,0.1496573,0.6294762],[0.7574398,0.3947192,0.1924464],[0.1498318,0.2409873,0.8137141],[0.6432346,0.2676371,0.9583846],[0.8517047,0.6855458,0.5413228],[0.4412634,0.751499,0.7884656],[0.5939756,0.9972243,0.3435504],[0.1283589,0.7168143,0.4213458],[0.308209,0.06448657,0.9809136],[0.361118,0.6180246,0.5825453],[0.2410122,0.4567871,0.8035313],[0.92677,0.7004799,0.6307325],[0.159856,0.8010102,0.06564418],[0.7632383,0.6643394,0.5151525],[0.2555983,0.5712122,0.6697811],[0.3976715,0.1158252,0.05177924],[0.5740297,0.7586238,0.1467678],[0.5367217,0.7372069,0.04172584],[0.7212515,0.2783328,0.7478626],[0.1802856,0.245905,0.7077527],[0.6376799,0.885767,0.3647935],[0.7762479,0.9089336,0.6902051],[0.4503707,0.9911413,0.3136873],[0.2393965,0.812039,0.4650568],[0.1756934,0.06197349,0.008356333],[0.9626651,0.6203654,0.6132363],[0.8257303,0.2194321,0.2688665],[0.4551463,0.2222483,0.1631426],[0.26008,0.4686306,0.8440437],[0.6408656,0.2775612,0.1929819]],"ignoreExtent":false,"flags":0},"5":{"id":5,"type":"light","vertices":[[0,0,1]],"colors":[[1,1,1,1],[1,1,1,1],[1,1,1,1]],"viewpoint":true,"finite":false},"4":{"id":4,"type":"background","colors":[[0.2980392,0.2980392,0.2980392,1]],"centers":[[0,0,0]],"sphere":false,"fogtype":"none"},"6":{"id":6,"type":"background","colors":[[1,1,1,1]],"centers":[[0,0,0]],"sphere":false,"fogtype":"none"},"8":{"id":8,"type":"bboxdeco","material":{"front":"lines","back":"lines"},"vertices":[[0.2,null,null],[0.4,null,null],[0.6,null,null],[0.8,null,null],[null,0.2,null],[null,0.4,null],[null,0.6,null],[null,0.8,null],[null,null,0.2],[null,null,0.4],[null,null,0.6],[null,null,0.8]],"colors":[[0,0,0,1]],"draw_front":true,"newIds":[20,21,22,23,24,25,26]},"1":{"id":1,"type":"subscene","par3d":{"antialias":8,"FOV":30,"ignoreExtent":false,"listeners":1,"mouseMode":{"left":"trackball","right":"zoom","middle":"fov","wheel":"pull"},"observer":[0,0,4.228135],"modelMatrix":[[0.999186,0,0,-0.5005143],[0,0.3429755,0.9378499,-0.6399861],[0,-0.9423175,0.3413495,-3.92833],[0,0,0,1]],"projMatrix":[[3.732051,0,0,0],[0,3.732051,0,0],[0,0,-3.863703,-15.24194],[0,0,-1,0]],"skipRedraw":false,"userMatrix":[[1,0,0,0],[0,0.3420201,0.9396926,0],[0,-0.9396926,0.3420201,0],[0,0,0,1]],"scale":[0.999186,1.002793,0.9980391],"viewport":{"x":0,"y":0,"width":1,"height":1},"zoom":1,"bbox":[0.002022203,0.999822,0.001212666,0.9972243,0.0003576479,0.9993041],"windowRect":[100,100,604,604],"family":"sans","font":1,"cex":1,"useFreeType":false,"fontname":"TT Arial","maxClipPlanes":8},"embeddings":{"viewport":"replace","projection":"replace","model":"replace"},"objects":[6,8,7,9,10,11,12,5,20,21,22,23,24,25,26],"subscenes":[],"flags":1192},"20":{"id":20,"type":"lines","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[0.2,-0.01372751,-0.01462655],[0.8,-0.01372751,-0.01462655],[0.2,-0.01372751,-0.01462655],[0.2,-0.03937481,-0.04034942],[0.4,-0.01372751,-0.01462655],[0.4,-0.03937481,-0.04034942],[0.6,-0.01372751,-0.01462655],[0.6,-0.03937481,-0.04034942],[0.8,-0.01372751,-0.01462655],[0.8,-0.03937481,-0.04034942]],"colors":[[0,0,0,1]],"centers":[[0.5,-0.01372751,-0.01462655],[0.2,-0.02655116,-0.02748798],[0.4,-0.02655116,-0.02748798],[0.6,-0.02655116,-0.02748798],[0.8,-0.02655116,-0.02748798]],"ignoreExtent":true,"origId":8,"flags":128},"21":{"id":21,"type":"text","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[0.2,-0.09066941,-0.09179516],[0.4,-0.09066941,-0.09179516],[0.6,-0.09066941,-0.09179516],[0.8,-0.09066941,-0.09179516]],"colors":[[0,0,0,1]],"texts":[["0.2"],["0.4"],["0.6"],["0.8"]],"cex":[[1]],"adj":[[0.5,0.5]],"centers":[[0.2,-0.09066941,-0.09179516],[0.4,-0.09066941,-0.09179516],[0.6,-0.09066941,-0.09179516],[0.8,-0.09066941,-0.09179516]],"family":[["sans"]],"font":[[1]],"ignoreExtent":true,"origId":8,"flags":40},"22":{"id":22,"type":"lines","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[-0.01294479,0.2,-0.01462655],[-0.01294479,0.8,-0.01462655],[-0.01294479,0.2,-0.01462655],[-0.03863814,0.2,-0.04034942],[-0.01294479,0.4,-0.01462655],[-0.03863814,0.4,-0.04034942],[-0.01294479,0.6,-0.01462655],[-0.03863814,0.6,-0.04034942],[-0.01294479,0.8,-0.01462655],[-0.03863814,0.8,-0.04034942]],"colors":[[0,0,0,1]],"centers":[[-0.01294479,0.5,-0.01462655],[-0.02579147,0.2,-0.02748798],[-0.02579147,0.4,-0.02748798],[-0.02579147,0.6,-0.02748798],[-0.02579147,0.8,-0.02748798]],"ignoreExtent":true,"origId":8,"flags":128},"23":{"id":23,"type":"text","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[-0.09002482,0.2,-0.09179516],[-0.09002482,0.4,-0.09179516],[-0.09002482,0.6,-0.09179516],[-0.09002482,0.8,-0.09179516]],"colors":[[0,0,0,1]],"texts":[["0.2"],["0.4"],["0.6"],["0.8"]],"cex":[[1]],"adj":[[0.5,0.5]],"centers":[[-0.09002482,0.2,-0.09179516],[-0.09002482,0.4,-0.09179516],[-0.09002482,0.6,-0.09179516],[-0.09002482,0.8,-0.09179516]],"family":[["sans"]],"font":[[1]],"ignoreExtent":true,"origId":8,"flags":40},"24":{"id":24,"type":"lines","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[-0.01294479,-0.01372751,0.2],[-0.01294479,-0.01372751,0.8],[-0.01294479,-0.01372751,0.2],[-0.03863814,-0.03937481,0.2],[-0.01294479,-0.01372751,0.4],[-0.03863814,-0.03937481,0.4],[-0.01294479,-0.01372751,0.6],[-0.03863814,-0.03937481,0.6],[-0.01294479,-0.01372751,0.8],[-0.03863814,-0.03937481,0.8]],"colors":[[0,0,0,1]],"centers":[[-0.01294479,-0.01372751,0.5],[-0.02579147,-0.02655116,0.2],[-0.02579147,-0.02655116,0.4],[-0.02579147,-0.02655116,0.6],[-0.02579147,-0.02655116,0.8]],"ignoreExtent":true,"origId":8,"flags":128},"25":{"id":25,"type":"text","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[-0.09002482,-0.09066941,0.2],[-0.09002482,-0.09066941,0.4],[-0.09002482,-0.09066941,0.6],[-0.09002482,-0.09066941,0.8]],"colors":[[0,0,0,1]],"texts":[["0.2"],["0.4"],["0.6"],["0.8"]],"cex":[[1]],"adj":[[0.5,0.5]],"centers":[[-0.09002482,-0.09066941,0.2],[-0.09002482,-0.09066941,0.4],[-0.09002482,-0.09066941,0.6],[-0.09002482,-0.09066941,0.8]],"family":[["sans"]],"font":[[1]],"ignoreExtent":true,"origId":8,"flags":40},"26":{"id":26,"type":"lines","material":{"lit":false,"front":"lines","back":"lines"},"vertices":[[-0.01294479,-0.01372751,-0.01462655],[-0.01294479,1.012164,-0.01462655],[-0.01294479,-0.01372751,1.014288],[-0.01294479,1.012164,1.014288],[-0.01294479,-0.01372751,-0.01462655],[-0.01294479,-0.01372751,1.014288],[-0.01294479,1.012164,-0.01462655],[-0.01294479,1.012164,1.014288],[-0.01294479,-0.01372751,-0.01462655],[1.014789,-0.01372751,-0.01462655],[-0.01294479,-0.01372751,1.014288],[1.014789,-0.01372751,1.014288],[-0.01294479,1.012164,-0.01462655],[1.014789,1.012164,-0.01462655],[-0.01294479,1.012164,1.014288],[1.014789,1.012164,1.014288],[1.014789,-0.01372751,-0.01462655],[1.014789,1.012164,-0.01462655],[1.014789,-0.01372751,1.014288],[1.014789,1.012164,1.014288],[1.014789,-0.01372751,-0.01462655],[1.014789,-0.01372751,1.014288],[1.014789,1.012164,-0.01462655],[1.014789,1.012164,1.014288]],"colors":[[0,0,0,1]],"centers":[[-0.01294479,0.4992185,-0.01462655],[-0.01294479,0.4992185,1.014288],[-0.01294479,-0.01372751,0.4998309],[-0.01294479,1.012164,0.4998309],[0.5009221,-0.01372751,-0.01462655],[0.5009221,-0.01372751,1.014288],[0.5009221,1.012164,-0.01462655],[0.5009221,1.012164,1.014288],[1.014789,0.4992185,-0.01462655],[1.014789,0.4992185,1.014288],[1.014789,-0.01372751,0.4998309],[1.014789,1.012164,0.4998309]],"ignoreExtent":true,"origId":8,"flags":128}},"snapshot":"data:image/png;base64,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","width":505,"height":505,"sphereVerts":{"material":[],"it":[[0,6,7,19,4,8,6,22,2,7,8,25,7,6,8,26,0,7,9,27,2,10,7,24,5,9,10,32,9,7,10,33,0,11,6,18,3,12,11,37,4,6,12,39,6,11,12,40,0,9,11,34,5,13,9,31,3,11,13,44,11,9,13,45,1,14,15,47,2,8,14,49,4,15,8,21,15,14,8,52,1,16,14,46,5,10,16,55,2,14,10,29,14,16,10,57,1,15,17,58,4,12,15,51,3,17,12,36,17,15,12,62,1,17,16,53,3,13,17,61,5,16,13,42,16,17,13,65],[18,20,19,18,21,23,22,21,24,26,25,24,20,23,26,20,19,28,27,19,29,30,24,29,31,33,32,31,28,30,33,28,34,35,18,34,36,38,37,36,22,40,39,22,35,38,40,35,27,41,34,27,42,43,31,42,37,45,44,37,41,43,45,41,46,48,47,46,25,50,49,25,51,52,21,51,48,50,52,48,53,54,46,53,32,56,55,32,49,57,29,49,54,56,57,54,47,59,58,47,39,60,51,39,61,62,36,61,59,60,62,59,58,63,53,58,44,64,61,44,55,65,42,55,63,64,65,63],[19,18,20,20,22,21,23,23,25,24,26,26,26,20,23,23,27,19,28,28,24,29,30,30,32,31,33,33,33,28,30,30,18,34,35,35,37,36,38,38,39,22,40,40,40,35,38,38,34,27,41,41,31,42,43,43,44,37,45,45,45,41,43,43,47,46,48,48,49,25,50,50,21,51,52,52,52,48,50,50,46,53,54,54,55,32,56,56,29,49,57,57,57,54,56,56,58,47,59,59,51,39,60,60,36,61,62,62,62,59,60,60,53,58,63,63,61,44,64,64,42,55,65,65,65,63,64,64]],"vb":[[-1,1,0,0,0,0,-0.7071068,-0.7071068,0,-0.7071068,0,-0.7071068,0,0,0.7071068,0.7071068,0.7071068,0.7071068,-0.9349975,-0.9349975,-0.77044,0,-0.3546542,-0.4507894,-0.3546542,0,-0.4507894,-0.9349975,-0.77044,0,-0.4507894,-0.3546542,0,-0.4507894,-0.9349975,-0.77044,0,-0.3546542,-0.4507894,0,-0.4507894,-0.77044,0,-0.4507894,0,-0.4507894,0.9349975,0.9349975,0.77044,0.3546542,0.4507894,0.3546542,0.4507894,0.9349975,0.77044,0.3546542,0.4507894,0.4507894,0.9349975,0.77044,0.4507894,0.3546542,0.4507894,0.77044,0.4507894,0.4507894],[0,0,-1,1,0,0,0,-0.7071068,-0.7071068,0,-0.7071068,0.7071068,0.7071068,0.7071068,-0.7071068,0,0,0.7071068,0,-0.3546542,-0.4507894,-0.3546542,0,-0.4507894,-0.9349975,-0.9349975,-0.77044,0,-0.4507894,-0.9349975,-0.77044,0,-0.3546542,-0.4507894,0.3546542,0.4507894,0.9349975,0.9349975,0.77044,0.3546542,0.4507894,0.4507894,0.3546542,0.4507894,0.9349975,0.77044,-0.3546542,0,-0.4507894,-0.9349975,-0.77044,0,-0.4507894,0,-0.4507894,0,-0.4507894,-0.77044,0.3546542,0.4507894,0.4507894,0.9349975,0.77044,0.4507894,0.77044,0.4507894],[0,0,0,0,-1,1,-0.7071068,0,-0.7071068,0.7071068,0.7071068,0,-0.7071068,0.7071068,0,-0.7071068,0.7071068,0,-0.3546542,0,-0.4507894,-0.9349975,-0.9349975,-0.77044,0,-0.3546542,-0.4507894,0.3546542,0.4507894,0.3546542,0.4507894,0.9349975,0.9349975,0.77044,0,-0.4507894,-0.3546542,0,-0.4507894,-0.9349975,-0.77044,0.4507894,0.9349975,0.77044,0.3546542,0.4507894,0,-0.3546542,-0.4507894,0,-0.4507894,-0.9349975,-0.77044,0.3546542,0.4507894,0.9349975,0.77044,0.4507894,0,-0.4507894,-0.77044,0,-0.4507894,0.4507894,0.4507894,0.77044]],"primitivetype":"triangle"}});
plot3drgl.prefix = "plot3d";
</script>
<p id="plot3ddebug">
You must enable Javascript to view this page properly.</p>
<script>plot3drgl.start();</script>

In the three dimensional space, spotting the outliers is somewhat harder but through focusing on the intervals we specified and rotating the graphic one can identify the areas where the outliers are visible.

For the symmetric case, the subspaces are of the following form:


```
## [1] 1 2
## [1] 3 4
## [1] 5 6
## [1] 7 8
## [1]  9 10
## [1] 11 12
## [1] 13 14
## [1] 15 16 17
## [1] 18 19 20
## [1] 21 22 23
## [1] 24 25 26
## [1] 27 28 29 30
## [1] "number of generated outliers:  47"
## [1] "number of generated subspaces:  12"
```

![plot of chunk plot2d-2](../images/blogpost-plot2d-2-1.png)


We also specified non-relevant dimensions of these data sets. They are of the following form.

![plot of chunk plot2d-nonRelevant](../images/blogpost-plot2d-nonRelevant-1.png)

Those highly correlated low-dimensional subspaces are not revealing any additional outliers. They might, however, influence the detection of the relevant subspaces by dominating less-correlated but more important subspaces.


### Source Code

The code for creating data sets and the markup for this blogpost are [available on github](https://github.com/holtri/dataGenerator).

