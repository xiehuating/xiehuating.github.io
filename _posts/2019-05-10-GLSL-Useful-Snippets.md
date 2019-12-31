---
layout: post
title: "GLSL代码片段"
description: "本文中收集GLSL常用的代码片段和造型函数。将这些封装好的函数复制到vertexShader和fragmentShader中，然后在main()中通过函数名直接调用。"
date: 2019-12-04
tags: [ CSS ]
comments: false
share: false
---

本文中收集GLSL常用的代码片段和造型函数。将这些封装好的函数复制到vertexShader和fragmentShader中，然后在main()中通过函数名直接调用。

## Snippets from shdr.bkcore.com

**DefaultVertex**

```c
precision highp float;
attribute vec3 position;
attribute vec3 normal;
uniform mat3 normalMatrix;
uniform mat4 modelViewMatrix;
uniform mat4 projectionMatrix;
varying vec3 fNormal;
varying vec3 fPosition;

void main()
{
  fNormal = normalize(normalMatrix * normal);
  vec4 pos = modelViewMatrix * vec4(position, 1.0);
  fPosition = pos.xyz;
  gl_Position = projectionMatrix * pos;
}
```

<br/>

**DefaultFragment**

```c
precision highp float;
uniform float time;
uniform vec2 resolution;
varying vec3 fPosition;
varying vec3 fNormal;

void main()
{
  gl_FragColor = vec4(fNormal, 1.0);
}
```

<br/>

**DemoVertex**

```c
precision highp float;
attribute vec3 position;

void main()
{
  gl_Position = vec4(position, 1.0);
}
```

<br/>

**DemoFragment**

```c
precision highp float;
uniform float time;
uniform vec2 resolution;
uniform mat4 modelViewMatrix;
uniform mat4 projectionMatrix;

void main()
{
  vec2 pixel = -1.0 + 2.0 * gl_FragCoord.xy / resolution.xy;
  pixel.x *= resolution.x/resolution.y;
  gl_FragColor = vec4(pixel,.0,1.);
}
```

<br/>

**ExtractCameraPos**

```c
vec3 ExtractCameraPos(mat4 a_modelView)
{
  mat3 rotMat =mat3(a_modelView[0].xyz,a_modelView[1].xyz,a_modelView[2].xyz);
  vec3 d =  a_modelView[3].xyz;
  vec3 retVec = -d * rotMat;
  return retVec;
}
```

<br/>

**GetDirection**

```c++
vec3 getDirection(vec3 origine, vec2 pixel)
{
  vec3 ww = normalize(vec3(0.0) - origine);
  vec3 uu = normalize(cross( vec3(0.0,1.0,0.0), ww ));
  vec3 vv = normalize(cross(ww,uu));
  return normalize( pixel.x*uu + pixel.y*vv + 1.5*ww );
}
```

<br/>

**Luma**

```c++
vec3 luma = vec3(0.299, 0.587, 0.114);
```

<br/>

**Fresnel**

```c++
float fresnel(float costheta, float fresnelCoef)
{
  return fresnelCoef + (1. - fresnelCoef) * pow(1. - costheta, 5.);
}
```

<br/>

**Ashikhmin(Dir)**

```c
float Ashikhmin(vec3 lightDir, vec3 viewDir,  vec3 normal, float exponent, float fresnelCoef)
{
  vec3 H = normalize(lightDir+viewDir);
  float numerateur_s = ( exponent + 1.)/(8.*3.14159) * pow(dot(normal,H), exponent );
  float denominateur_s = dot(lightDir,H)*(dot(normal,lightDir) + dot(normal, viewDir) - dot(normal, lightDir) * dot(normal, viewDir));
  float K =  fresnel(dot(normal,lightDir), fresnelCoef) * ( numerateur_s / denominateur_s  ) ;
  return K;
}
```

<br/>

**Blinn-Phong(Dir)**

```c
vec2 blinnPhongDir(vec3 lightDir, float lightInt, float Ka, float Kd, float Ks, float shininess)
{
  vec3 s = normalize(lightDir);
  vec3 v = normalize(-fPosition);
  vec3 n = normalize(fNormal);
  vec3 h = normalize(v+s);
  float diffuse = Ka + Kd * lightInt * max(0.0, dot(n, s));
  float spec =  Ks * pow(max(0.0, dot(n,h)), shininess);
  return vec2(diffuse, spec);
}
```

<br/>

**OrenNayard(Dir)**

```c
float OrenNayarDir(vec3 lightDir, vec3 viewDir, vec3 normal, float exponent)
{
  float LdotN = dot(lightDir,normal);
  float VdotN = dot(viewDir,normal);
  float result = clamp( LdotN, 0. , 1.);
  float soft_rim = clamp( 1. - VdotN/2., 0. , 1.);
  float fakey = pow(1. - result * soft_rim , 2.);
  float fakey_magic = 0.62;
  fakey = fakey_magic - fakey*fakey_magic;
  float K =  mix(result, fakey, exponent) ;
  return K;
}
```

<br/>

**Ward(Dir)**

```c
float Ward(vec3 lightDir, vec3 viewDir, vec3 normal, float exponent)
{
    vec3 H = normalize(lightDir + viewDir);
    float delta = acos(dot(H,normal));
    float alpha2 = exponent * exponent;
    float temp = exp(-pow(tan(delta), 2.) / (alpha2)) / (4. * 3.1415 * alpha2);
    float temp2 = sqrt(dot(viewDir,normal) * dot(lightDir,normal));
    float K = temp2 * temp;
    return K;
}
```

<br/>

**ColorNormal**

```c
vec3 colorNormal(vec3 col1, vec3 col2, vec3 col3)
{
  vec3 n = normalize(fNormal);
  return clamp(col1*n.x + col2*n.y + col3*n.z,
              vec3(0.0), vec3(1.0));
}
```

<br/>

**Rimlight**

```c
vec3 rim(vec3 color, float start, float end, float coef)
{
  vec3 normal = normalize(fNormal);
  vec3 eye = normalize(-fPosition.xyz);
  float rim = smoothstep(start, end, 1.0 - dot(normal, eye));
  return clamp(rim, 0.0, 1.0) * coef * color;
}
```

<br/>

**Split**

```c
vec3 split(vec3 left, vec3 right, float ratio, bool horizontal)
{
  float i = float(horizontal);
  float m = i*gl_FragCoord.x/resolution.x;
  m += (1.0-i)*gl_FragCoord.y/resolution.y;
  float d = float(m < ratio);
  return left*d + right*(1.0-d);
}
```

<br/>

**Transpose(mat3)**

```
mat3 transpose( mat3 m )
{
  mat3 ret = m;
  ret[0][1] = m[1][0];
  ret[0][2] = m[2][0];
  ret[1][0] = m[0][1];
  ret[1][2] = m[2][1];
  ret[2][0] = m[0][2];
  ret[2][1] = m[1][2];
  return ret;
}
```

<br/>

## Shaping functions | 造型函数

<br/>

### Useful little functions from [Iñigo Quiles](http://www.iquilezles.org/)

<http://www.iquilezles.org/www/articles/functions/functions.htm>

**Almost Identity**

![](/images/glsl-useful-snippets/useful-little-functions/almost-identity.png)

```c
float almostIdentity( float x, float m, float n )
{
    if( x>m ) return x;
    const float a = 2.0*n - m
    const float b = 2.0*m - 3.0*n;
    const float t = x/m;
    return (a*t + b)*t*t + n;
}
```

<br/>

**Exponential Impulse**

![](/images/glsl-useful-snippets/useful-little-functions/exponential-impulse.png)

```c
float expImpulse( float k, float x ){
    float h = k*x;
    return h*exp(1.0-h);
}
```

<br/>

**Polynomial Impulse**

![](/images/glsl-useful-snippets/useful-little-functions/polynomial-impulse.png)

```c
float quaImpulse( float k, float x )
{
    return 2.0*sqrt(k)*x/(1.0+k*x*x);
}
```

<br/>

**Cubic Pulse**

![](/images/glsl-useful-snippets/useful-little-functions/cubic-pulse.png)

```c
float cubicPulse( float c, float w, float x ){
    x = abs(x - c);
    if( x>w ) return 0.0;
    x /= w;
    return 1.0 - x*x*(3.0-2.0*x);
}
```

<br/>

**Exponential Step**

![](/images/glsl-useful-snippets/useful-little-functions/exponential-step.png)

```c
float expStep( float x, float k, float n ){
    return exp( -k*pow(x,n) );
}
```

<br/>

**Gain**

![](/images/glsl-useful-snippets/useful-little-functions/gain.png)

```c
float gain(float x, float k) 
{
    const float a = 0.5*pow(2.0*((x<0.5)?x:1.0-x), k);
    return (x<0.5)?a:1.0-a;
}
```

<br/>

**Parabola**

![](/images/glsl-useful-snippets/useful-little-functions/parabola.png)

```c
float parabola( float x, float k ){
    return pow( 4.0*x*(1.0-x), k );
}
```

<br/>

**Power curve**

![](/images/glsl-useful-snippets/useful-little-functions/power-curve.png)

```c
float pcurve( float x, float a, float b ){
    float k = pow(a+b,a+b) / (pow(a,a)*pow(b,b));
    return k * pow( x, a ) * pow( 1.0-x, b );
}
```

<br/>

**Sinc curve**

![](/images/glsl-useful-snippets/useful-little-functions/sinc-curve.png)

```c
float sinc( float x, float k )
{
    const float a = PI*((k*x-1.0);
    return sin(a)/a;
}
```

<br/>

### Table of equations from [Kynd](http://www.kynd.info/log/) 

[![](/images/glsl-useful-snippets/kynd.png)](/images/glsl-useful-snippets/kynd.png)

<br/>

## Easing functions for transition| 缓动函数过渡

Robert Penner's easing functions in GLSL

<https://easings.net/en>

<https://github.com/stackgl/glsl-easings>

<https://thebookofshaders.com/edit.php#06/easing.frag>

<br/>

**Linear**

```c
float linear(float t) {
  return t;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/01sine.jpg)

**easeInSine**

```c
float sineIn(float t) {
  return sin((t - 1.0) * HALF_PI) + 1.0;
}
```

**easeOutSine**

```c
float sineOut(float t) {
  return sin(t * HALF_PI);
}
```

**easeInOutSine**

```c
float sineInOut(float t) {
  return -0.5 * (cos(PI * t) - 1.0);
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/02quad.jpg)

**easeInQuad**

```c
float quadraticIn(float t) {
  return t * t;
}
```

**easeOutQuad**

```c
float quadraticInOut(float t) {
  float p = 2.0 * t * t;
  return t < 0.5 ? p : -p + (4.0 * t) - 1.0;
}
```

**easeInOutQuad**

```c
float quadraticOut(float t) {
  return -t * (t - 2.0);
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/03cubic.jpg)

**easeInCubic**

```c
float cubicIn(float t) {
  return t * t * t;
}
```

**easeOutCubic**

```c
float cubicOut(float t) {
  float f = t - 1.0;
  return f * f * f + 1.0;
}
```

**easeInOutCubic**

```c
float cubicInOut(float t) {
  return t < 0.5
    ? 4.0 * t * t * t
    : 0.5 * pow(2.0 * t - 2.0, 3.0) + 1.0;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/04quart.jpg)

**easeInQuart**

```c
float quarticIn(float t) {
  return pow(t, 4.0);
}
```

**easeOutQuart**

```c
float quarticOut(float t) {
  return pow(t - 1.0, 3.0) * (1.0 - t) + 1.0;
}
```

**easeInOutQuart**

```c
float quarticInOut(float t) {
  return t < 0.5
    ? +8.0 * pow(t, 4.0)
    : -8.0 * pow(t - 1.0, 4.0) + 1.0;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/05Quint.jpg)

**easeInQuint**

```c
float qinticIn(float t) {
  return pow(t, 5.0);
}
```

**easeOutQuint**

```c
float qinticOut(float t) {
  return 1.0 - (pow(t - 1.0, 5.0));
}
```

**easeInOutQuint**

```c
float qinticInOut(float t) {
  return t < 0.5
    ? +16.0 * pow(t, 5.0)
    : -0.5 * pow(2.0 * t - 2.0, 5.0) + 1.0;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/06expo.jpg)

**easeInExpo**

```c
float exponentialIn(float t) {
  return t == 0.0 ? t : pow(2.0, 10.0 * (t - 1.0));
}
```

**easeOutExpo**

```c
float exponentialOut(float t) {
  return t == 1.0 ? t : 1.0 - pow(2.0, -10.0 * t);
}
```

**easeInOutExpo**

```c
float exponentialInOut(float t) {
  return t == 0.0 || t == 1.0
    ? t
    : t < 0.5
      ? +0.5 * pow(2.0, (20.0 * t) - 10.0)
      : -0.5 * pow(2.0, 10.0 - (t * 20.0)) + 1.0;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/07circ.jpg)

**easeInCirc**

```c
float circularIn(float t) {
  return 1.0 - sqrt(1.0 - t * t);
}
```

**easeOutCirc**

```c
float circularOut(float t) {
  return sqrt((2.0 - t) * t);
}
```

**easeInOutCirc**

```c
float circularInOut(float t) {
  return t < 0.5
    ? 0.5 * (1.0 - sqrt(1.0 - 4.0 * t * t))
    : 0.5 * (sqrt((3.0 - 2.0 * t) * (2.0 * t - 1.0)) + 1.0);
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/08back.jpg)

**easeInBack**

```c
float backIn(float t) {
  return pow(t, 3.0) - t * sin(t * PI);
}
```

**easeOutBack**

```c
float backOut(float t) {
  float f = 1.0 - t;
  return 1.0 - (pow(f, 3.0) - f * sin(f * PI));
}
```

**easeInOutBack**

```c
float backInOut(float t) {
  float f = t < 0.5
    ? 2.0 * t
    : 1.0 - (2.0 * t - 1.0);

  float g = pow(f, 3.0) - f * sin(f * PI);

  return t < 0.5
    ? 0.5 * g
    : 0.5 * (1.0 - g) + 0.5;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/09elastic.jpg)

**easeInElastic**

```c
float elasticIn(float t) {
  return sin(13.0 * t * HALF_PI) * pow(2.0, 10.0 * (t - 1.0));
}
```

**easeOutElastic**

```c
float elasticOut(float t) {
  return sin(-13.0 * (t + 1.0) * HALF_PI) * pow(2.0, -10.0 * t) + 1.0;
}
```

**easeInOutElastic**

```c
float elasticInOut(float t) {
  return t < 0.5
    ? 0.5 * sin(+13.0 * HALF_PI * 2.0 * t) * pow(2.0, 10.0 * (2.0 * t - 1.0))
    : 0.5 * sin(-13.0 * HALF_PI * ((2.0 * t - 1.0) + 1.0)) * pow(2.0, -10.0 * (2.0 * t - 1.0)) + 1.0;
}
```

<br/>

![](/images/glsl-useful-snippets/easing-functions/10bounce.jpg)

**easeInBounce**

```c
float bounceIn(float t) {
  return 1.0 - bounceOut(1.0 - t);
}
```

**easeOutBounce**

```c
float bounceOut(float t) {
  const float a = 4.0 / 11.0;
  const float b = 8.0 / 11.0;
  const float c = 9.0 / 10.0;

  const float ca = 4356.0 / 361.0;
  const float cb = 35442.0 / 1805.0;
  const float cc = 16061.0 / 1805.0;

  float t2 = t * t;

  return t < a
    ? 7.5625 * t2
    : t < b
      ? 9.075 * t2 - 9.9 * t + 3.4
      : t < c
        ? ca * t2 - cb * t + cc
        : 10.8 * t * t - 20.52 * t + 10.72;
}
```

**easeInOutBounce**

```c
float bounceInOut(float t) {
  return t < 0.5
    ? 0.5 * (1.0 - bounceOut(1.0 - t * 2.0))
    : 0.5 * bounceOut(t * 2.0 - 1.0) + 0.5;
}
```

<br/>

## How to use

### 在GLSL在线编辑器中调试

GLSL Shader online editer:

- <https://thebookofshaders.com/edit.php>
- <http://shdr.bkcore.com/>
- <http://glslsandbox.com/e>

### 在ThreeJS中使用



<br/>

## 更多资料

- **Shaping functions from** [**Golan Levin**](http://www.flong.com/) 
  - [**多项式造型函数**](http://www.flong.com/texts/code/shapers_poly/)
  - [**指数造型函数**](http://www.flong.com/texts/code/shapers_exp/)
  - [**圆与椭圆造型函数**](http://www.flong.com/texts/code/shapers_circ/)
  - [**贝塞尔和其他参数化造型函数**](http://www.flong.com/texts/code/shapers_bez/)
- [**Iñigo Quiles‘ articles**](http://www.iquilezles.org/www/index.htm)
- [**webgl-noise**](https://github.com/ashima/webgl-noise)



