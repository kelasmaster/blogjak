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

The complete code:

```
 const POSTS_PER_PAGE = 6;

    function loadElement(url, elementId) {
      return fetch(url)
        .then(response => {
          if (!response.ok) throw new Error(`Error loading ${url}`);
          return response.text();
        })
        .then(html => {
          document.getElementById(elementId).innerHTML = html;
        })
        .catch(error => {
          console.error(error);
          document.getElementById(elementId).innerHTML = 
            `<div class="error">Error loading ${elementId}</div>`;
        });
    }

    Promise.all([
      loadElement('header.html', 'header'),
      loadElement('footer.html', 'footer'),
      fetch('/pelatihan/posts.json').then(response => response.json())
    ]).then(([_, __, posts]) => {
      document.getElementById('year').textContent = new Date().getFullYear();

      const currentPage = parseInt(new URLSearchParams(window.location.search).get('page')) || 1;
      const totalPages = Math.ceil(posts.length / POSTS_PER_PAGE);
      const startIndex = (currentPage - 1) * POSTS_PER_PAGE;
      const paginatedPosts = posts.slice(startIndex, startIndex + POSTS_PER_PAGE);

      const postList = document.getElementById('gallery-item');
      postList.innerHTML = paginatedPosts.map(post => `
         <div class="gallery-item">  
         <a href="${post.url}"><img src="${post.img}" alt="${post.alt}" loading="lazy"></a>
         </div>
      `).join('');

      const pagination = document.getElementById('pagination');
      pagination.innerHTML = `
        <button onclick="changePage(${currentPage - 1})" ${currentPage === 1 ? 'disabled' : ''}>Newer</button>
        ${Array.from({length: totalPages}, (_, i) => i + 1).map(page => `
          <button onclick="changePage(${page})" ${currentPage === page ? 'class="active"' : ''}>${page}</button>
        `).join('')}
        <button onclick="changePage(${currentPage + 1})" ${currentPage === totalPages ? 'disabled' : ''}>Older</button>
      `;
    }).catch(error => console.error(error));

    window.changePage = (page) => {
      const url = new URL(window.location);
      url.searchParams.set('page', page);
      window.history.pushState({}, '', url);
      location.reload();
    };

```
