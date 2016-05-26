Picasso

## 缓存

- get
- set
- size
- clear

### Files

```java
Cache.java
LruCache.java
```

### 内存缓存策略

## 下载

- load
- shutdown

### Files

```java
Downloader.java
UrlConnectionDownloader.java
OkHttpDownloader.java
OkHttp3Downloader.java
```

### 网络请求策略

## 请求

图片Uri、图片配置等

```java
----------------
| imageUri     |
| targetWidth  |
| targetHeight |
| ... 		   |
----------------
		|
		|
		|
		|
		V
---------------
|  Request    |
--------------- 
```

`Builder模式`

## 结果

`RequestHandler.Result`

```java
----------------------------
| Bitmap                   |
| LoadedFrom[Bitmap来源]    |
| InputStream              |
| exifOrientation          |
----------------------------
			|
			|
			V
-----------------------------
|	RequestHandler.Result	|
-----------------------------
```

## 请求Handler

- Result load(Request request, int networkPolicy) throws IOException;

```java
-----------------				-----------------------------
|	Request 	|----(load)--->	|	RequestHandler.Result	|
-----------------	  			-----------------------------
```

### Files

```java
AssetRequestHandler.java
ContactsPhotoRequestHandler.java
ContentStreamRequestHandler.java
FileRequestHandler.java
MediaStoreRequestHandler.java
NetworkRequestHandler.java
ResourceRequestHandler.java
```

## Action

## Dispatch