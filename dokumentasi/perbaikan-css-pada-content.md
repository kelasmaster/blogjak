# Perbaikan CSS Pada Content
Ada dua masalah ketika melihat tampilan postingan yakni

1. Title post tidak di tengah
2. Gambar tidak responsif
3. Link footer dengan warna berbeda

Semua sudah saya update dengan menambah ccs:
```
.content h2 {
  text-align: center;
}
.content img {
  width: 100%;
  height: auto;
}
footer a {
  color: white;
}
```
