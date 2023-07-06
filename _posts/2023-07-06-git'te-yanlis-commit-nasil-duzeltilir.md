---
layout: mypost
title: Git'te Yanlış Commit Nasıl Düzeltilir?
categories: [Git,3. Türkçe]
tags: [Git, Gitlab, Github]
---

Git deposundaki commit bilgilerini düzeltmeniz gerektiğinde, aşağıdaki adımları izleyin:

**Not**: `git filter-branch` kullanarak commit geçmişini değiştirmek ve değişiklikleri zorla itmek ciddi sonuçlar doğurabilir. İlerlemeden önce dikkatli olmak ve sonuçları anlamak önemlidir.

`git filter-branch` kullanarak değişiklikleri zorla itmek, birkaç sonuca yol açabilir:

- **Commit Geçmişinin Kaybı**: Commit bilgilerini değiştirmek, farklı meta verileri olan yeni commit'ler oluşturur, eski commit'leri etkili bir şekilde değiştirir ve commit geçmişini yeniden yazmış olur. Bu, orijinal commit'leri ve ilişkili bilgilerini takip etmeyi zorlaştırabilir.

- **İşbirliği Zorlukları**: Commit geçmişini yeniden yazmak, birden fazla kişinin aynı kod tabanında işbirliği yaparken senkronizasyon sorunlarına neden olabilir. Diğer işbirlikçiler, çalışmalarını orijinal commit'ler üzerine dayandırmış olabilir, bu da güncellenmiş geçmişle çalışmalarını birleştirirken çatışmalara ve zorluklara yol açabilir.

- **Entegrasyon Bozulması**: Commit geçmişini değiştirmek, sürekli entegrasyon (CI) pipeline'ları, hata takipçileri ve kod inceleme sistemleri gibi çeşitli araçlar ve hizmetlerle entegrasyonu bozabilir. Bu entegrasyonlar, düzgün çalışma için orijinal commit'ler ve meta verilerine güvenir.

Yanlış commit bilgilerini düzeltmek için şu adımları izleyin:

1. Terminalinizi açın ve aşağıdaki komutu çalıştırın:
```bash
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="<hatalı_email>"
CORRECT_NAME="<doğru_isim>"
CORRECT_EMAIL="<doğru_eposta>"
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

2. Komutu çalıştırdıktan sonra, güncellenmiş commit'leri ve etiketleri uzak depoya zorla itmek için aşağıdaki komutu çalıştırın:
```bash
git push --force --tags origin 'refs/heads/*'
```

Bu adımları takip ederek, Git deposundaki yanlış commit bilgilerini düzeltebilirsiniz. `<hatalı_email>`, `<doğru_isim>` ve `<doğru_eposta>` değerlerini kendi durumunuza uygun şekilde değiştirin.