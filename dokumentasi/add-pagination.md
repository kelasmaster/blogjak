Update Add Pagination

HTML
```
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Home - My Blog</title>
  <link rel="stylesheet" href="styles.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
</head>
<body>
  <div id="header"></div>
  
  <div class="content">
    <h2>Latest Posts</h2>
    <div id="post-list" class="post-list"></div>
    <div id="pagination" class="pagination"></div>
  </div>

  <div id="footer"></div>

  <script>
    // Configuration
    const POSTS_PER_PAGE = 5;

    // Load common elements
    Promise.all([
      fetch('header.html').then(res => res.text()),
      fetch('footer.html').then(res => res.text()),
      fetch('posts.json').then(res => res.json())
    ]).then(([header, footer, posts]) => {
      document.getElementById('header').innerHTML = header;
      document.getElementById('footer').innerHTML = footer;
      document.getElementById('year').textContent = new Date().getFullYear();
      
      // Pagination logic
      const currentPage = parseInt(new URLSearchParams(window.location.search).get('page')) || 1;
      const totalPages = Math.ceil(posts.length / POSTS_PER_PAGE);
      const startIndex = (currentPage - 1) * POSTS_PER_PAGE;
      const endIndex = startIndex + POSTS_PER_PAGE;
      const paginatedPosts = posts.slice(startIndex, endIndex);

      // Render posts
      const postList = document.getElementById('post-list');
      postList.innerHTML = paginatedPosts.map(post => `
        <div class="post-item">
          <h3><a href="${post.url}">${post.title}</a></h3>
          <p>${post.date}</p>
          <p>${post.excerpt}</p>
        </div>
      `).join('');

      // Render pagination
      const pagination = document.getElementById('pagination');
      pagination.innerHTML = `
        <button onclick="changePage(${currentPage - 1})" ${currentPage === 1 ? 'disabled' : ''}>Newer</button>
        ${Array.from({length: totalPages}, (_, i) => i + 1).map(page => `
          <button onclick="changePage(${page})" ${currentPage === page ? 'class="active"' : ''}>${page}</button>
        `).join('')}
        <button onclick="changePage(${currentPage + 1})" ${currentPage === totalPages ? 'disabled' : ''}>Older</button>
      `;
    });

    // Pagination handler
    window.changePage = (page) => {
      const url = new URL(window.location);
      url.searchParams.set('page', page);
      window.history.pushState({}, '', url);
      location.reload();
    };
  </script>
</body>
</html>
```

JSON
```
<!-- posts.json -->
[
  {
    "title": "My First Blog Post",
    "date": "January 1, 2023",
    "excerpt": "This is a short excerpt from the blog post...",
    "url": "posts/post1.html"
  },
  {
    "title": "Second Post",
    "date": "February 1, 2023",
    "excerpt": "Another example blog post excerpt...",
    "url": "posts/post2.html"
  }
  // Add more posts here
]
```

CSS
```
/* Added to styles.css */
.pagination {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 2rem 0;
}

.pagination button {
  background: #333;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  cursor: pointer;
}

.pagination button.active {
  background: #555;
}

.pagination button:disabled {
  background: #999;
  cursor: not-allowed;
}
```

HTML
```
<!-- posts/post2.html (example new post) -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Second Post</title>
  <link rel="stylesheet" href="../styles.css">
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
</head>
<body>
  <div id="header"></div>
  
  <div class="content">
    <div id="post-content"></div>
  </div>

  <div id="footer"></div>

  <script>
    // Load header and footer
    fetch('../header.html')
      .then(response => response.text())
      .then(data => {
        document.getElementById('header').innerHTML = data;
      });
    fetch('../footer.html')
      .then(response => response.text())
      .then(data => {
        document.getElementById('footer').innerHTML = data;
        document.getElementById('year').textContent = new Date().getFullYear();
      });

    // Load and render markdown
    fetch('post2.md')
      .then(response => response.text())
      .then(text => {
        document.getElementById('post-content').innerHTML = 
          marked.parse(text);
      });
  </script>
  <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
</body>
</html>
```
Key features added:

Dynamic pagination system with:
Previous/Newer and Next/Older buttons
Page number navigation
URL parameter-based page tracking
posts.json file for post management
Automatic post list generation
Responsive pagination controls
Page state preservation in URL
To use:

Add all posts to posts.json with:
Title
Date
Excerpt
URL to post HTML file
Create new post HTML files in /posts directory
Adjust POSTS_PER_PAGE constant in index.html as needed
Test pagination by visiting /index.html?page=2 etc.
New posts automatically appear in pagination when added to posts.json
