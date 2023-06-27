# BQAPI Services

## Image Service

The image service has the following endpoints:

{% code overflow="wrap" %}
```xml
<resource uri="/image_service">
<method name="/image_service/operations" value="Returns a list of supported operations in XML"/>
<method name="/image_service/formats" value="Returns a list of supported formats in XML"/>
<method name="/image_service/ID" value="Returns a file for this ID"/>
<method name="/image_service/ID?OPERATION1=PAR1&OPERATION2=PAR2" value="Executes operations for give image ID. Call /operations to check available"/>
<method name="/image_service/ID/OPERATION1:PAR1/OPERATION2:PAR2" value="Executes operations for give image ID. Call /operations to check available"/>
<method name="/image_service/image/ID" value="same as /ID"/>
<method name="/image_service/images/ID" value="same as /ID, deprecated and will be removed in the future"/>
</resource>
```
{% endcode %}

### API for Operations on an Image

{% swagger method="get" path="/roi" baseUrl="/image_service/operations" summary="Returns an image in specified ROI" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" required="true" type="x1,y1,x2,y2[;x1,y1,x2,y2]" name="roi" %}
All values are in ranges 

`[1..N]`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/brightnesscontrast" baseUrl="/image_service/operations" summary="Adjust brightness and contrast in an image" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="brightnesscontrast" type="[-100,100]" required="true" %}
`brightnesscontrast=brightness,contrast both in range [-100,100]`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/textureatlas" baseUrl="/image_service/operations" summary="Returns a 2D texture atlas image for a given 3D input" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="textureatlas" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/transform" baseUrl="/image_service/operations" summary="Returns a transformed image" %}
{% swagger-description %}
`transform=fourier|chebyshev|wavelet|radon|edge|wndchrmcolor|rgb2hsv|hsv2rgb|superpixels`
{% endswagger-description %}

{% swagger-parameter in="query" name="transform" required="true" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/histogram" baseUrl="/image_service/operations" summary="Returns a histogram of an image" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="histogram" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/fuse" baseUrl="/image_service/operations" summary="Returns an RGB/Gray image with the requested channel fusion" %}
{% swagger-description %}
`arg=[W1R,W1G,W1B;W2R,W2G,W2B;...[:METHOD]][display][gray]`
{% endswagger-description %}

{% swagger-parameter in="query" name="fuse" %}

{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

{% swagger method="get" path="/intensityprojection" baseUrl="/image_service/operations" summary="Returns a maximum intensity projection image" %}
{% swagger-description %}

{% endswagger-description %}

{% swagger-parameter in="query" name="intensityprojection" required="true" %}
`intensityprojection=max|min`
{% endswagger-parameter %}

{% swagger-response status="200: OK" description="" %}
```javascript
{
    // Response
}
```
{% endswagger-response %}
{% endswagger %}

## Table Service

{% swagger method="get" path="/table" baseUrl="https://bisque2.ece.ucsb.edu" summary="Executes operations for a given table ID." %}
{% swagger-description %}
Typically used for getting slice from HDF5 data.&#x20;

**Example**  A `numpy` array stored in an HDF5 that is too big to load into memory on a Macbook. We only want to process chunks of the data at a time.
{% endswagger-description %}

{% swagger-parameter in="query" name="id" required="true" %}
`/table//ID[/PATH1/PATH2/...][/RANGE][/COMMAND:PARS]`
{% endswagger-parameter %}
{% endswagger %}









