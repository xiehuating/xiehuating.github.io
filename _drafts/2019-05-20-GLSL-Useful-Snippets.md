---
layout: post
title: "GLSL代码片段"
description: ""
date: 2019-05-20
tags: [ webgl, glsl ]
comments: false
share: false
---



# GLSL代码片段

## shdr.bkcore.com

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

**DemoVertex**

```c
precision highp float;
attribute vec3 position;

void main()
{
  gl_Position = vec4(position, 1.0);
}
```

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

**Luma**

```c++
vec3 luma = vec3(0.299, 0.587, 0.114);
```

**Fresnel**

```c++
float fresnel(float costheta, float fresnelCoef)
{
  return fresnelCoef + (1. - fresnelCoef) * pow(1. - costheta, 5.);
}
```

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



**ColorNormal**

```c
vec3 colorNormal(vec3 col1, vec3 col2, vec3 col3)
{
  vec3 n = normalize(fNormal);
  return clamp(col1*n.x + col2*n.y + col3*n.z,
              vec3(0.0), vec3(1.0));
}
```



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



## Easing functions | 缓动函数





Robert Penner's easing functions in GLSL

https://easings.net/en

https://github.com/stackgl/glsl-easings

https://thebookofshaders.com/edit.php#06/easing.frag



```c
float linear(float t) {
  return t;
}
```



```c
float exponentialIn(float t) {
  return t == 0.0 ? t : pow(2.0, 10.0 * (t - 1.0));
}
```





```c
float exponentialOut(float t) {
  return t == 1.0 ? t : 1.0 - pow(2.0, -10.0 * t);
}
```



```c
float exponentialInOut(float t) {
  return t == 0.0 || t == 1.0
    ? t
    : t < 0.5
      ? +0.5 * pow(2.0, (20.0 * t) - 10.0)
      : -0.5 * pow(2.0, 10.0 - (t * 20.0)) + 1.0;
}
```





```c
float sineIn(float t) {
  return sin((t - 1.0) * HALF_PI) + 1.0;
}
```





```c
float sineOut(float t) {
  return sin(t * HALF_PI);
}
```





```c
float sineInOut(float t) {
  return -0.5 * (cos(PI * t) - 1.0);
}
```





```c
float qinticIn(float t) {
  return pow(t, 5.0);
}
```





```c
float qinticOut(float t) {
  return 1.0 - (pow(t - 1.0, 5.0));
}
```





```c
float qinticInOut(float t) {
  return t < 0.5
    ? +16.0 * pow(t, 5.0)
    : -0.5 * pow(2.0 * t - 2.0, 5.0) + 1.0;
}
```





```c
float quarticIn(float t) {
  return pow(t, 4.0);
}
```





```c
float quarticOut(float t) {
  return pow(t - 1.0, 3.0) * (1.0 - t) + 1.0;
}
```





```c
float quarticInOut(float t) {
  return t < 0.5
    ? +8.0 * pow(t, 4.0)
    : -8.0 * pow(t - 1.0, 4.0) + 1.0;
}
```





```c
float quadraticInOut(float t) {
  float p = 2.0 * t * t;
  return t < 0.5 ? p : -p + (4.0 * t) - 1.0;
}
```





```c
float quadraticIn(float t) {
  return t * t;
}
```





```c
float quadraticOut(float t) {
  return -t * (t - 2.0);
}
```





```c
float cubicIn(float t) {
  return t * t * t;
}
```





```c
float cubicOut(float t) {
  float f = t - 1.0;
  return f * f * f + 1.0;
}
```





```c
float cubicInOut(float t) {
  return t < 0.5
    ? 4.0 * t * t * t
    : 0.5 * pow(2.0 * t - 2.0, 3.0) + 1.0;
}
```





```c
float elasticIn(float t) {
  return sin(13.0 * t * HALF_PI) * pow(2.0, 10.0 * (t - 1.0));
}
```





```c
float elasticOut(float t) {
  return sin(-13.0 * (t + 1.0) * HALF_PI) * pow(2.0, -10.0 * t) + 1.0;
}
```





```c
float elasticInOut(float t) {
  return t < 0.5
    ? 0.5 * sin(+13.0 * HALF_PI * 2.0 * t) * pow(2.0, 10.0 * (2.0 * t - 1.0))
    : 0.5 * sin(-13.0 * HALF_PI * ((2.0 * t - 1.0) + 1.0)) * pow(2.0, -10.0 * (2.0 * t - 1.0)) + 1.0;
}
```





```c
float circularIn(float t) {
  return 1.0 - sqrt(1.0 - t * t);
}
```





```c
float circularOut(float t) {
  return sqrt((2.0 - t) * t);
}
```





```c
float circularInOut(float t) {
  return t < 0.5
    ? 0.5 * (1.0 - sqrt(1.0 - 4.0 * t * t))
    : 0.5 * (sqrt((3.0 - 2.0 * t) * (2.0 * t - 1.0)) + 1.0);
}
```





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





```c
float bounceIn(float t) {
  return 1.0 - bounceOut(1.0 - t);
}
```





```c
float bounceInOut(float t) {
  return t < 0.5
    ? 0.5 * (1.0 - bounceOut(1.0 - t * 2.0))
    : 0.5 * bounceOut(t * 2.0 - 1.0) + 0.5;
}
```





```c
float backIn(float t) {
  return pow(t, 3.0) - t * sin(t * PI);
}
```





```c
float backOut(float t) {
  float f = 1.0 - t;
  return 1.0 - (pow(f, 3.0) - f * sin(f * PI));
}
```





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

















## 参考资料



