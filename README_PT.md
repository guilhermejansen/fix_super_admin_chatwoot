# Fix Super Admin Chatwoot - README

## 📌 Sobre este Tutorial (Portugues)

Este tutorial explica passo a passo como criar e confirmar um **SuperAdmin** no **Chatwoot** após a implantação quando a criação inicial do usuário falha. Ele cobre métodos para **Docker, Portainer e instalações locais**.

This tutorial explains step by step how to create and confirm a **SuperAdmin** user in **Chatwoot** after deployment when the initial user creation fails. It covers methods for **Docker, Portainer, and local host setups**.

🎥 **Tutorial em vídeo no YouTube:** [Clique aqui para assistir](https://youtu.be/RTBqgbCFxGA)

---

## 🚀 Comandos para corrigir o SuperAdmin

### 🐳 **Usando Docker**
#### 1️⃣ Abra um terminal e execute:
```bash
docker exec -it $(docker ps --filter name=srv-captain--chatwoot-web -q) /bin/sh
```
Este comando entra no container do Chatwoot.

#### 2️⃣ Inicie o console Rails:
```bash
RAILS_ENV=production bundle exec rails c
```

#### 3️⃣ Crie um novo **SuperAdmin**:
```ruby
SuperAdmin.create!(
  email: 'admin@example.com',
  password: 'Str0ngP@ssw0rd!',
  name: 'Admin Chatwoot'
)
```
> ⚠️ **Use uma senha forte com números, maiúsculas, minúsculas e caracteres especiais.**

#### 4️⃣ Confirme o usuário manualmente:
```ruby
admin = SuperAdmin.find_by(email: 'admin@example.com')
admin.confirmed_at = Time.now
admin.confirmation_token = nil
admin.save!
```

#### 5️⃣ Saia do console e reinicie o Chatwoot:
```bash
exit
docker restart $(docker ps --filter name=srv-captain--chatwoot-web -q)
```
Agora, acesse o Chatwoot em:
```
https://seu-dominio.com/super_admin
```

---

### 🖥️ **Usando Portainer**
1. Abra o **Portainer** e navegue até o **container do Chatwoot**.
2. Clique em **Console** e selecione **/bin/sh**.
3. Dentro do terminal, execute:
   ```bash
   RAILS_ENV=production bundle exec rails c
   ```
4. Siga os mesmos passos **3️⃣, 4️⃣ e 5️⃣** do guia Docker.

---

### 🏠 **Para instalação local**
#### 1️⃣ Abra um terminal no diretório do Chatwoot e execute:
```bash
RAILS_ENV=production bundle exec rails c
```

#### 2️⃣ Crie e confirme o SuperAdmin:
```ruby
SuperAdmin.create!(
  email: 'admin@example.com',
  password: 'Str0ngP@ssw0rd!',
  name: 'Admin Chatwoot'
)
admin = SuperAdmin.find_by(email: 'admin@example.com')
admin.confirmed_at = Time.now
admin.confirmation_token = nil
admin.save!
```

#### 3️⃣ Reinicie o Chatwoot:
```bash
sudo systemctl restart chatwoot
```
Agora, acesse:
```
https://localhost:3000/super_admin
```

---

## 🛠 Solução de Problemas
### ❌ **Usuário não encontrado?**
Verifique se o usuário existe:
```ruby
SuperAdmin.all
```
Se estiver vazio, crie o usuário novamente.

### ❌ **Não consegue fazer login?**
Redefina a senha:
```ruby
admin = SuperAdmin.find_by(email: 'admin@example.com')
admin.password = 'NewP@ssword123!'
admin.save!
```

---

## ✅ Conclusão
Agora, você tem um **SuperAdmin** funcional no Chatwoot! 🚀 Se encontrar problemas, verifique os logs e certifique-se de que o banco de dados está em execução.

🔗 **Assista ao tutorial em vídeo aqui:** [Clique aqui](https://youtu.be/RTBqgbCFxGA)

---

_Este guia foi projetado para desenvolvedores e administradores de sistemas implantando instâncias auto-hospedadas do Chatwoot._

