# Change blog post into responsive image grid post

CSS

```
.gallery-item {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
}

.gallery-item img {
  width: 100%;
}
```

Posts Json - Remove date

```
[
   {
    "alt": "Alkholisi Plain Detergent: Sabun Cair Khusus untuk Keperluan Haji dan Umroh 250 ml",  
    "img": "https://ratakan.com/uploads/prd-89ed7719e8.jpg",
    "url": "posts/plain-detergent-sabun-cair-250ml.html"
  },
   {
    "alt": "Alkholisi Plain Detergent Sabun Cair Pilihan Tepat untuk Pakaian Haji dan Umroh",  
    "img": "https://ratakan.com/uploads/prd-14342c8582.jpg",
    "url": "posts/plain-detergent-sabun-cair-1l.html"
  },
  {
    "alt": "Plain Detergent Sabun Bubuk 500 Gram",
    "img": "https://ratakan.com/uploads/prd-2b06068c1d.jpg",
    "url": "posts/plain-detergent-sabun-bubuk-500g.html"
  }
]
```

Index
Change This

```
 <div class="post-item">
          <h3><a href="${post.url}">${post.title}</a></h3>
          <p>${post.date}</p>
          <p>${post.excerpt}</p>
        </div>
```

Into:

```
<div class="gallery-item">  
         <a href="${post.url}"><img src="${post.img}" alt="${post.alt}" loading="lazy"></a>
         </div>
```

Change this

```
 <div id="post-list" class="post-list"></div>
 const postList = document.getElementById('post-list');
```

Into:
```
 <div id="post-list" class="gallery-item"></div>
 const postList = document.getElementById('gallery-item');
```
