# Fix Super Admin Chatwoot - README

## 📌 About this Tutorial (English & Portuguese)

This tutorial explains step by step how to create and confirm a **SuperAdmin** user in **Chatwoot** after deployment when the initial user creation fails. It covers methods for **Docker, Portainer, and local host setups**.

Este tutorial explica passo a passo como criar e confirmar um usuário **SuperAdmin** no **Chatwoot** após o deploy, caso a criação inicial do usuário falhe. Ele cobre métodos para **Docker, Portainer e hospedagem local**.

🎥 **YouTube Video Tutorial:** [Click here to watch](https://your-video-link.com)

---

## 🚀 Commands to Fix SuperAdmin

### 🐳 **Using Docker**
#### 1️⃣ Open a terminal and run:
```bash
docker exec -it $(docker ps --filter name=srv-captain--chatwoot-web -q) /bin/sh
```
This command enters the Chatwoot container.

#### 2️⃣ Start the Rails console:
```bash
RAILS_ENV=production bundle exec rails c
```

#### 3️⃣ Create a new **SuperAdmin** user:
```ruby
SuperAdmin.create!(
  email: 'admin@example.com',
  password: 'Str0ngP@ssw0rd!',
  name: 'Admin Chatwoot'
)
```
> ⚠️ **Use a strong password with numbers, uppercase, lowercase, and special characters.**

#### 4️⃣ Confirm the user manually:
```ruby
admin = SuperAdmin.find_by(email: 'admin@example.com')
admin.confirmed_at = Time.now
admin.confirmation_token = nil
admin.save!
```

#### 5️⃣ Exit the console and restart Chatwoot:
```bash
exit
docker restart $(docker ps --filter name=srv-captain--chatwoot-web -q)
```
Now, access Chatwoot at:
```
https://your-domain.com/super_admin
```

---

### 🖥️ **Using Portainer**
1. Open **Portainer** and navigate to your **Chatwoot container**.
2. Click on **Console** and select **/bin/sh**.
3. Inside the terminal, run:
   ```bash
   RAILS_ENV=production bundle exec rails c
   ```
4. Follow the same steps **3️⃣, 4️⃣, and 5️⃣** as in the Docker guide.

---

### 🏠 **For Local Host Setup**
#### 1️⃣ Open a terminal in the Chatwoot directory and run:
```bash
RAILS_ENV=production bundle exec rails c
```

#### 2️⃣ Create and confirm the SuperAdmin:
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

#### 3️⃣ Restart Chatwoot:
```bash
sudo systemctl restart chatwoot
```
Now, access:
```
https://localhost:3000/super_admin
```

---

## 🛠 Troubleshooting
### ❌ **User Not Found?**
Check if the user exists:
```ruby
SuperAdmin.all
```
If empty, create the user again.

### ❌ **Cannot Login?**
Reset the password:
```ruby
admin = SuperAdmin.find_by(email: 'admin@example.com')
admin.password = 'NewP@ssword123!'
admin.save!
```

---

## ✅ Conclusion
Now, you have a working **SuperAdmin** in Chatwoot! 🚀 If you encounter issues, check the logs and ensure the database is running.

🔗 **Watch the tutorial video here:** [Click here](https://your-video-link.com)

---
