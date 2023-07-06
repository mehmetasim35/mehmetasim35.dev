---
layout: mypost
title: Cara Memperbaiki Informasi Committer yang Salah dalam Git
categories: [Git,4.Bahasa Indonesia]
tags: [Git, Gitlab, Github, Commit]
---

Jika Anda perlu memperbaiki informasi committer yang salah dalam repositori Git Anda, ikuti langkah-langkah berikut:

**Catatan**: Memodifikasi sejarah commit menggunakan `git filter-branch` dan memaksa mendorong perubahan dapat memiliki konsekuensi yang signifikan. Penting untuk berhati-hati dan memahami implikasinya sebelum melanjutkan.

Menggunakan `git filter-branch` dan memaksa mendorong perubahan dapat memiliki beberapa konsekuensi:

- **Kehilangan Sejarah Commit**: Memodifikasi informasi commit membuat commit baru dengan metadata yang berbeda, efektif menggantikan commit lama dan menulis ulang sejarah commit. Hal ini dapat membuat sulit untuk melacak commit asli dan informasi terkaitnya.

- **Tantangan Kolaborasi**: Menulis ulang sejarah commit dapat menyebabkan masalah sinkronisasi ketika beberapa orang bekerja sama pada kode yang sama. Kolaborator lain mungkin telah memulai pekerjaan mereka berdasarkan commit asli, yang menyebabkan konflik dan kesulitan menggabungkan pekerjaan mereka dengan sejarah yang diperbarui.

- **Gangguan Integrasi**: Memodifikasi sejarah commit dapat mengganggu integrasi dengan berbagai alat dan layanan seperti continuous integration (CI) pipelines, pelacak isu, dan sistem review kode. Integrasi ini bergantung pada commit asli dan metadata mereka untuk berfungsi dengan baik.

Untuk memperbaiki informasi committer yang salah, ikuti langkah berikut:

1. Buka terminal Anda dan jalankan perintah berikut:
```bash
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="<email_salah>"
CORRECT_NAME="<nama_benar>"
CORRECT_EMAIL="<email_benar>"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

2. Setelah menjalankan perintah tersebut, jalankan perintah berikut untuk memaksa mendorong commit dan tag yang diperbarui ke repositori remote Anda:
```bash
git push --force --tags origin 'refs/heads/*'
```

Dengan mengikuti langkah-langkah ini, Anda akan memperbaiki informasi committer yang salah dalam repositori Git Anda. Pastikan Anda menggantikan `<email_salah>`, `<nama_benar>`, dan `<email_benar>` dengan nilai yang sesuai dengan situasi Anda.